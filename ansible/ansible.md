# Ansible  
## Introducción a Ansible  
>
Aquí voy a explicar para que sirve el `inventory.ini`.
Este es un diccionario donde guardaremos las direcciones
IP de una maquina y le asignaremos un nombre y también mas
parámetros que nos permite tener las maquinas ordenadas.
>
## inventory.ini
>
En el archivo `inventory.ini` podemos poner todos los equipos
que vayamos a usar en la ejecucion de un `playbook.yaml`.
>
* Parámetros  
`[GRUPO]` los grupos son agrupaciones de hosts que nos 
permitirá ejecutar acciones con muchos equipos a la vez.  
`ansible_host` este nos permitirá determinar el nombre para
una maquina en el inventario.  
`ansible_user` es nos permitirá conectarnos con un usuario 
determinado hacia la maquina.    

Ejemplo
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
un nombre a la función que va a realizar

```
- name: nombre que le vamos a dar a nuestro playbook
```
después podemos darle parámetros al inicio del playbook
para por ejemplo definir a quien va dirigido y demás.
Estos son unos ejemplos:

```
hosts: [grupo] #define a que grupo del diccionario va dirigido
become: true | false #define si puede usar sudo sin necesidad de pedirlo
vars: aqui podemos definir las variables que se van a usar en nuestro playbook  
tasks: #este campo se rellenaria con toda la tarea que va a realizar el playbook
```

### tasks

> Aquí es donde se define todos los procesos que se realizaran en la maquina.
> 
**Tenemos varios campos que podemos definir aquí**

Ejemplos
> Podemos usar `when:` en cualquier función para hacer una comprobación que queramos
es como usar un if.  
También podemos usar `ignore_errors: yes` para que si hay un error en la ejecución 
de la función no pare el playbook y continue.
Podemos usar `become: yes` en una task para que se ejecute con permisos de sudo.
>
```
- name: uso de comando shell en el playbook
  ansible.builtin.command: "<comando>"
# estos parametros se pueden usar en varias tareas
  changed_when: false # <-- sirve para evitar que Ansible marque una tarea como “changed” aunque haya tenido salida.
  failed_when: false # <-- sirve para evitar que una tarea falle, incluso si el comando devuelve un error.
  check_mode: yes # <-- sirve para ver qué haría una tarea sin cambiar el sistema. Equivalente a ejecutar el playbook con --check, pero solo para esa tarea.
  creates: # <-- Evita ejecutar una tarea si ya existe un archivo.
  removes: # <-- Evita ejecutar la tarea si el archivo NO existe.

- name: uso de script en el playbook
  ansible.builtin.shell: | # <-- aqui puedes usar la | o > esto indicara como interaciona el playbook con los saltos de linea  
        if id "{{ usuario }}" &>dev/null; then
          sudo userdel "{{ usuario }}"
        fi

- name: intalar paquete #para poner varios paquetes pondiamos despuede de "name:" un salto de linea y empezarimos a poner -<nombre-paquete> salto de linea -<nombre-paquete>
      package:
        name: cpp
        #name: 
          #- <nombre-paquete>
          #- <nombre-paquete> 
        state: present # <-- este parametro comprueba que este instalado cuando acabe la funcion.  
        update_cache: true # <-- esto sirve para que actualize base de datos de paquetes antes de instalar nada.

- name: iniciar y habilitar un proceso 
  systemd:
    name: <servicio>
    state: started
    enabled: yes  
```
También podemos crear archivos y editarlos con Ansible

* Crear archivos de texto
```
- name: Crear archivo de texto
  copy:
    dest: /tmp/mi_archivo.txt
    content: "Este es el contenido del archivo\n"
- name: Crear archivo vacío
  file:
    path: /tmp/vacio.txt
    state: touch
```
* Editar archivos de texto
```
- name: Añadir línea a un archivo si existe la Reemplazar
  lineinfile:
    path: /tmp/mi_archivo.txt
    line: "Nueva línea añadida"

- name: Reemplazar texto en un archivo
  replace:
    path: /tmp/mi_archivo.txt
    regexp: "antiguo"
    replace: "nuevo"

# Tambien podemos añadir en bloque el contenido
- name: Insertar un bloque de texto
  blockinfile:
    path: /tmp/mi_archivo.txt
    block: |
      Esta es una sección nueva
      contenida en un bloque.
```
* Crear archivos mediate un template
```
#podemos crear un archivo mediante un template 
- name: Desplegar config desde plantilla y hacer backup del archivo anterior
  ansible.builtin.template:
    src: templates/mi_config.extension
    dest: /etc/mi_app/mi_config.conf
    owner: root          # <-- asignamos a quien le pertenece el archivo
    group: root          # <-- asignamos a que grupo le pertenece el archivo
    mode: '0644'         # <-- permisos y bits especiales
    backup: yes          # <-- crea un backup del archivo destino antes de sobrescribir
```
