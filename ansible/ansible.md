# Ansible  
## Introduccion a ansible  
>
Aqui voy a explicar para que sirve el `inventory.ini`.
Este es un diccionario donde guardaremos las direcciones
IP de una maquina y le asignaremos un nombre y tambien mas
parametros que nos permite tener las maquinas ordenadas.
>
## inventory.ini
>
En el archivo `inventory.ini` podemos poner todos los equipos
que vayamos a usar en la ejecucion de un `playbook.yaml`.
>
* Parametros  
`[GRUPO]` los grupos son agrupaciones de hosts que nos 
permitira ejecutar acciones con muchos equipos a la vez.  
`ansible_host` este nos permitira determinar el nombre para
una maquina en el inventario.  
`ansible_user` es nos permitira conectarnos con un usuario 
determinado hacia la maquina.    

ejemplo
```yaml
[myhosts]
ejemplo         ansible_host=192.168.10.5       ansible_user=usuario_ejemplo
```  
## playbook.yaml

>
En el archivo `playbook.yaml` este archivo es un documento
donde se va a programar una tarea para que se ejecute con
los equipos que tenemos en el inventario. **Se puede llamar
como quieras**
>

Para iniciar el archivo playbook.yaml primero tienes que darle
un nombre a la funcion que va a realizar

```
- name: nombre que le vamos a dar a nuestro playbook
```
despues podemos darle parametros al inicio del playbook
para por ejemplo definir a quien va dirigido y demas.
Estos son unos ejemplos:

```
hosts: [grupo] #define a que grupo del diccionario va dirigido
become: true | false #define si puede usar sudo sin necesidad de pedirlo
vars: aqui podemos definir las variables que se van a usar en nuestro playbook  
tasks: #este campo se rellenaria con toda la tarea que va a realizar el playbook
```

### tasks

> Aqui es donde se define todos los procesos que se realizaran en la maquina.
> 
**Tenemos varios campos que podemos definir aqui**

ejemplos
> Podemos usar `when:` en cualquier funcion para hacer una comprabacion que queramos
es como usar un if.  
Tambien podemos usar `ignore_errors: yes` para que si hay un error en la ejecucion 
de la funcion no pare el playbook y continue.
>
```
- name: uso de comando shell en el playbook
  ansible.builtin.command: "<comando>"

- name: uso de script en el playbook
  ansible.builtin.shell: | #aqui puedes usar la | o > esto indicara como interaciona el playbook con los saltos de linea  
        if id "{{ usuario }}" &>dev/null; then
          sudo userdel "{{ usuario }}"
        fi

- name: intalar paquete #para poner varios paquetes pondiamos despuede de "name:" un salto de linea y empezarimos a poner -<nombre-paquete> salto de linea -<nombre-paquete>
      package:
        name: cpp
        #name: 
          #- <nombre-paquete>
          #- <nombre-paquete> 
        state: present #este parametro comprueba que este instalado cuando acabe la funcion.  

- name: iniciar y habilitar un proceso 
  systemd:
    name: <servicio>
    state: started
    enabled: yes  
```
