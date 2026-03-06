# Proyecto SFTP

## Infraestructura

vamos a tener 4 maquinas, dos van a funcionar como servidores sftp, 1 como servidor nfs y la ultima 
que va a ser el cliente de los dos servidores sftp.

Para esto vamos a necesitar instalar en el cliente y los dos servidores el paquete `openssh`, y en los dos servidores sftp y en el servidor nfs el paquete `nfs-utils`.

## Instalacion NFS

### SERVER
He creado mi carpeta con el nombre **sftp**, le damos permisos con el comando `chmod 755 /sftp`.
Despues de esto vamos a editar el archivo `/etc/exports` y vamos a poner nuestra carpeta a compartir
y la red en la que la vamos a compartir tanto los permisos que vamos a tener.
`/sftp  <ip.de.nuestra.red/mascara.de.nuestra.red>(<permisos>)`, yo en los permisos he puesto `rw,sync,no_subtree_check`.
Hecho esto podemos arrancar el servicio nfs, `sudo systemctl enable --now nfs-server` y aplicamos los cambios
del archivo exports `sudo exports -rav`.
Yo he tenido que abrir el firewall para el nfs.
```
sudo firewall-cmd --permanent --add-service=nfs
sudo firewall-cmd --reload
```

### Cliente
Para el cliente yo edite el archivo `/etc/fstab` y aqui añadi mi carpeta, ponemos 
`<ip.servidor>:<ruta.carpeta> <ruta.montaje> nfs defaults 0 0` y despues hacemos `sudo mount -a`

## INSTALACION SFTP

### SERVER
Primero necesitamos un usuario donde no tenga acceso a shell, para esto lo crearemos, tambien necesitamos 
un grupo al que pertenecera el usuario.
```
sudo groupadd sftp
sudo adduser -m -G sftp -s /sbin/nologin <usuario>
sudo passwd <usuario>
```
ahora dentro de la carpeta nfs, crearemos una carpeta para el usuario y le daremos a la carpeta el grupo
del usuario `sudo chown root:sftp <carpeta.a.la.que.queremos.restringir.los.permisos>`

despues tendremos que editar el archivo `/etc/ssh/sshd_config` y añadir al final
```
Subsystem sftp internal-sftp -l INFO

Match Group sftp
    ChrootDirectory <carpeta.donde.vamos.a.enjaular.al.usuario>
    ForceCommand internal-sftp -l INFO -f LOCAL6
    AllowTcpForwarding no
    X11Forwarding no
```
Ahora necesitamos habilitar el acceso
```
sudo semanage fcontext -a -t ssh_home_t "<ruta.enjaulamiento.usuarios>(/.*)?"
sudo restorecon -Rv /sftp
```
despues reiniciamos el servicio `sudo systemctl restart sshd`

Para que haga los logs correctamente tendremos que configurar **rsyslog**.
añadir al archivo `/etc/rsyslog.d/sftp.conf` si no exite el archivo lo creamos y le ponemos dentro
```
input(type="imuxsock" Socket="<ruta.enjaulamiento.usuario>/dev/log" CreatePath="on")
local6.*        /var/log/sftp.log
& stop
```
y habilitar los socks locales en `/etc/rsyslog.conf` cambiando el parametro `SysSock.Use="off"` a `SysSock.Use="on"`.
despues de esto vamos a habilitar que nuestro rsyslog pueda crear sockets
```
sudo semanage fcontext -a -t syslogd_var_run_t "<ruta.enjaulamiento.usaurio>/dev(/.*)?"
sudo restorecon -Rv <ruta.enjaulamiento.usuario>/dev
```
despues de esto reiniciamos los servicios `sudo systemctl restart rsyslog.service` y `sudo systemctl restart sshd`
