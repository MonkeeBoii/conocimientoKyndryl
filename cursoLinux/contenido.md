# Curso Linux
## Indice

# Índice

## 1. Estructura del Sistema de Ficheros
- [Estructura de directorios de Linux](#estructura-de-directorios-de-linux)
  - [/](#-1)
  - [/bin](#bin)
  - [/sbin](#sbin)
  - [/usr](#usr)
  - [/lib](#lib)
  - [/lib64](#lib64)
  - [/etc](#etc)
  - [/home](#home)
  - [/root](#root)
  - [/opt](#opt)
  - [/var](#var)
  - [/tmp](#tmp)
  - [/mnt](#mnt)
  - [/media](#media)
  - [/dev](#dev)
  - [/proc](#proc)
  - [/sys](#sys)
  - [/run](#run)

## 2. Configuración y Gestión de Red
- [a) Interfaces de red](#a-interfaces-de-red)
- [b) Configuración de DNS](#b-configuración-de-dns)
- [c) Configuración de rutas de red](#c-configuración-de-rutas-de-red)
- [d) Firewall en Linux](#d-firewall-en-linux)
- [e) Conexiones y uso de puertos](#e-conexiones-y-uso-de-puertos)

## 3. Gestión de Usuarios
- [a) Usuarios y grupos de usuario](#a-usuarios-y-grupos-de-usuario)
- [b) Permisos y propietarios](#b-permisos-y-propietarios)

## 4. Servicios
- [a) SysVinit vs Systemd](#a-sysvinit-vs-systemd)
- [b) Servicio de acceso: SSH](#b-servicio-de-acceso-ssh)
- [c) Servicios de transferencia de ficheros: FTP, SFTP y SCP](#c-servicios-de-transferencia-de-ficheros-ftp-sftp-y-scp)

## 5. Herramientas de Administración
- [a) Logs del sistema (syslog, logrotate)](#a-logs-del-sistema-syslog-logrotate)
- [b) Procesos (ps, lsof, fuser)](#b-procesos-ps-lsof-fuser)
- [c) Monitorización de recursos](#c-monitorización-de-recursos-cpu-memoria-swap-red-y-disco)
- [d) Tareas programadas (cron)](#d-tareas-programadas-cron)
- [e) Búsqueda de ficheros y directorios (find)](#e-búsqueda-de-ficheros-y-directorios-find)
- [f) Copias y backup (cp, rsync, tar)](#f-copias-y-backup-de-ficheros-y-directorios-cp-rsync-tar)

## 6. Discos, Filesystems y LVM
- [a) Identificación de discos y multipath](#a-identificación-de-discos-y-multipath)
- [b) Configuración de particiones](#b-configuración-de-particiones)
- [c) Sistemas de ficheros](#c-sistemas-de-ficheros)
- [d) Gestión de LVM](#d-gestión-de-lvm-pvs-vgs-lvs-y-filesystems)

## 7. Gestión de Paquetes
- [a) Licencias](#a-licencias)
- [b) Repositorios y gestores de paquetes](#b-repositorios-y-gestores-de-paquetes)

---
## Introducción a Linux
### Estructura de directorios de Linux
#### /
Raíz del sistema. Desde aquí cuelgan todos los directorios.

#### /bin
Programas esenciales para todos los usuarios (ej: `ls`, `cp`, `mv`, `rm`).

#### /sbin
Programas esenciales de administración, normalmente usados por root.

#### /usr
Contiene la mayoría del software instalado:
- /usr/bin → programas no esenciales  
- /usr/sbin → utilidades administrativas  
- /usr/lib → librerías  
- /usr/share → archivos compartidos (iconos, docs, etc.)

#### /lib
Librerías esenciales requeridas por `/bin` y `/sbin`.

#### /lib64
Librerías esenciales para sistemas de 64 bits.

#### /etc
Archivos de configuración del sistema (servicios, red, usuarios, etc.).

#### /home
Carpetas personales de cada usuario.

#### /root
Carpeta personal del superusuario **root**.

#### /opt
Software opcional o de terceros (aplicaciones instaladas fuera del sistema base).

#### /var
Datos variables:
- Logs en `/var/log`
- Cachés
- Archivos de bases de datos temporales
- Colas, spool, etc.

#### /tmp
Archivos temporales generados por programas. Suele vaciarse al reiniciar.

#### /mnt
Punto de montaje manual para unidades externas o sistemas de archivos.

#### /media
Puntos de montaje automáticos (USB, DVD, SD cards).

#### /dev
Archivos de dispositivo que representan hardware (ej: `/dev/sda`, `/dev/null`, `/dev/tty`).

#### /proc
Sistema virtual que muestra información del kernel y de los procesos.

#### /sys
Sistema virtual similar a `/proc`, centrado en hardware y drivers.

#### /run
Información temporal generada después del arranque: PIDs, sockets, estados.

### Configuración y Gestión de Red

#### a) Interfaces de red
Comandos básicos para ver, activar y configurar interfaces:
- `ip addr` — muestra interfaces y direcciones IP  
- `ip link` — muestra el estado de las interfaces  
- `ip link set eth0 up/down` — activa/desactiva una interfaz  
- `ip a add 192.168.1.10/24 dev eth0` — asigna IP  
- `nmcli dev status` — gestión con NetworkManager  
- `ifconfig` — comando clásico (puede no venir instalado)

#### b) Configuración de DNS
Editar o verificar servidores DNS:
- `cat /etc/resolv.conf` — ver servidores DNS activos  
- `nmcli con mod <perfil> ipv4.dns "8.8.8.8"` — cambiar DNS en NetworkManager  
- `systemd-resolve --status` — ver DNS usando systemd-resolved  

#### c) Configuración de rutas de red
Comandos para ver y manipular la tabla de rutas:
- `ip route` — ver rutas  
- `ip route add default via 192.168.1.1` — añadir puerta de enlace  
- `ip route del 0.0.0.0/0` — eliminar ruta por defecto  
- `route -n` — comando clásico  

#### d) Firewall en Linux
Comandos esenciales según firewall:
**UFW (simple):**
- `ufw status` — ver reglas  
- `ufw enable/disable` — activar/desactivar  
- `ufw allow 22` — permitir puerto  

**FirewallD (sistemas basados en RHEL/Fedora):**
- `firewall-cmd --state`  
- `firewall-cmd --add-port=80/tcp --permanent`  
- `firewall-cmd --reload`  

#### e) Conexiones y uso de puertos
Ver conexiones activas y puertos en uso:
- `ss -tulpn` — ver puertos abiertos y procesos  
- `netstat -tulpn` — alternativa clásica  
- `lsof -i :80` — ver quién usa un puerto  
- `ping <host>` — probar conectividad  
- `traceroute <host>` — trazar ruta hasta un destino

### Gestión de Usuarios

#### a) Usuarios y grupos de usuario
Comandos básicos para crear, modificar y gestionar usuarios y grupos:
- `useradd <usuario>` — crea un usuario nuevo  
- `passwd <usuario>` — asigna o cambia contraseña  
- `userdel <usuario>` — elimina un usuario  
- `usermod -aG <grupo> <usuario>` — añade un usuario a un grupo  
- `groupadd <grupo>` — crea un grupo  
- `groupdel <grupo>` — elimina un grupo  
- `id <usuario>` — muestra UID, GID y grupos del usuario  
- `getent passwd` — lista usuarios del sistema  
- `getent group` — lista grupos del sistema  

#### b) Permisos y propietarios
Comandos para gestionar permisos, propietarios y modos de acceso:
- `ls -l` — muestra permisos, propietarios y grupos  
- `chmod 755 <archivo>` — cambia permisos (rwxr-xr-x)  
- `chmod u+x <archivo>` — añade permiso de ejecución al propietario  
- `chown <usuario>:<grupo> <archivo>` — cambia propietario y grupo  
- `chgrp <grupo> <archivo>` — cambia solo el grupo  
- `umask` — muestra/define permisos por defecto de nuevos archivos  

### Servicios

#### a) SysVinit vs Systemd
**SysVinit** (antiguo):
- `/etc/init.d/` — scripts de arranque  
- `service <servicio> start|stop|status`  
- `update-rc.d` — gestionar niveles de arranque  

**Systemd** (moderno y estándar actual):
- `systemctl start <servicio>` — iniciar servicio  
- `systemctl stop <servicio>` — detener  
- `systemctl enable <servicio>` — habilitar al arranque  
- `systemctl status <servicio>` — ver estado  
- `journalctl -u <servicio>` — ver logs del servicio

#### b) Servicio de acceso: SSH
Comandos básicos para gestionar y usar SSH:
- `systemctl start ssh` — iniciar servicio SSH  
- `systemctl enable ssh` — activar al arranque  
- `ssh usuario@host` — conexión remota  
- `ssh-keygen` — generar clave  
- `ssh-copy-id usuario@host` — copiar clave pública al servidor  
- Configuración: `/etc/ssh/sshd_config`

#### c) Servicios de transferencia de ficheros: FTP, SFTP y SCP
**FTP** (no cifrado):
- Servicio común: `vsftpd`  
- `systemctl start vsftpd`  
- Cliente: `ftp <host>`  

**SFTP** (cifrado, usa SSH):
- Cliente: `sftp usuario@host`  
- Comandos internos: `get`, `put`, `ls`, `cd`, `mkdir`

**SCP** (copia cifrada por SSH):
- `scp archivo usuario@host:/ruta/` — subir archivo  
- `scp usuario@host:/ruta/archivo .` — descargar  
- `scp -r carpeta usuario@host:/destino` — copiar recursivo

# Herramientas de Administración

#### a) Logs del sistema (syslog, logrotate)
Herramientas para ver y gestionar logs:
- `journalctl` — ver logs del sistema (Systemd)  
- `journalctl -u <servicio>` — logs de un servicio  
- `less /var/log/syslog` — ver syslog (Debian/Ubuntu)  
- `less /var/log/messages` — logs generales (CentOS/RHEL)  
- `logrotate -f /etc/logrotate.conf` — forzar rotación de logs  
- Configuración: `/etc/logrotate.d/`

#### b) Procesos (ps, lsof, fuser)
Comandos esenciales para inspección y control de procesos:
- `ps aux` — lista todos los procesos  
- `top` o `htop` — monitor de procesos en tiempo real  
- `lsof -i` — ver procesos usando la red  
- `lsof <archivo>` — ver quién usa un archivo  
- `fuser -v <archivo>` — identificar procesos que usan un archivo  
- `kill <PID>` — terminar proceso  
- `kill -9 <PID>` — terminar forzadamente  

#### c) Monitorización de recursos (CPU, memoria, swap, red y disco)
Comandos útiles para monitorizar:
- `top` / `htop` — CPU, RAM, cargas  
- `free -h` — memoria y swap  
- `vmstat` — procesos, memoria, interrupciones  
- `iostat` — uso de disco  
- `df -h` — espacio en disco  
- `du -sh *` — tamaño de carpetas  
- `iftop` o `ip -s link` — tráfico de red  
- `sar` — estadísticas del sistema (si está instalado)

#### d) Tareas programadas (cron)
Comandos para programar tareas automáticas:
- `crontab -e` — editar tareas del usuario  
- `crontab -l` — listar tareas  
- Archivo global: `/etc/crontab`  
- Directorios automáticos: `/etc/cron.daily`, `cron.hourly`, etc.

#### e) Búsqueda de ficheros y directorios (find)
Comandos esenciales:
- `find /ruta -name "*.txt"` — buscar por nombre  
- `find /ruta -type d` — buscar directorios  
- `find . -size +100M` — ficheros mayores a 100 MB  
- `find /ruta -mtime -1` — modificados en las últimas 24h  
- `locate <nombre>` — búsqueda rápida (usa base de datos)

#### f) Copias y Backup de ficheros y directorios (cp, rsync, tar)
Herramientas para copia y respaldo:
- `cp archivo destino/` — copiar fichero  
- `cp -r carpeta destino/` — copiar directorio  

**rsync (muy usado en backups):**
- `rsync -av origen/ destino/` — copia sincronizada  
- `rsync -avz origen usuario@host:/ruta/` — copia remota por SSH  

**tar (empaquetar y comprimir):**
- `tar -cvf archivo.tar carpeta/` — crear tar  
- `tar -czvf archivo.tar.gz carpeta/` — crear tar comprimido  
- `tar -xvf archivo.tar` — extraer

### Discos, Filesystems y LVM

#### a) Identificación de discos y multipath
Comandos para listar discos, particiones y rutas:
- `lsblk` — lista discos, particiones y LVM  
- `fdisk -l` — muestra discos y tabla de particiones  
- `blkid` — identifica UUID y tipo de sistema de ficheros  
- `cat /proc/partitions` — lista particiones detectadas  
- `multipath -ll` — ver dispositivos multipath (si multipath está configurado)  

#### b) Configuración de particiones
Crear y modificar particiones en discos:
- `fdisk /dev/sdX` — particionado MBR  
- `cfdisk /dev/sdX` — particionador interactivo  
- `parted /dev/sdX` — particionado GPT y avanzado  
- `mkfs.ext4 /dev/sdX1` — formatea una partición  
- `mkfs.xfs /dev/sdX1` — formatea con XFS  

#### c) Sistemas de ficheros
Gestión básica de filesystems:
- `mkfs.<tipo> <dispositivo>` — crear filesystem (ej.: ext4, xfs, vfat)  
- `fsck <dispositivo>` — comprobar/repárar filesystem  
- `mount <dev> <dir>` — montar un sistema de ficheros  
- `umount <dir>` — desmontar  
- `/etc/fstab` — archivo para montajes automáticos  

Ejemplos:
- `mkfs.ext4 /dev/sda1`  
- `mount /dev/sda1 /mnt`  

#### d) Gestión de LVM: PVs, VGs, LVs y filesystems
LVM se organiza en tres capas:


PV – Physical Volume
Son discos o particiones reales.
Ej: /dev/sda1, /dev/sdb.


VG – Volume Group
Es un “pool” de espacio que se forma uniendo varios PV.
Ej: miVG con 3 discos sumados.


LV – Logical Volume
De este “pool” creas volúmenes lógicos, que funcionan como particiones.
Ej: miVG/miLV → lo formateas y montas como si fuera una partición normal.

* Aumentar o reducir espacio fácilmente  
Puedes ampliar un LV sin apagar el sistema (si el FS lo permite).  
* Combinar varios discos como si fueran uno  
Ideal para servidores con discos añadidos con el tiempo.  
* Snapshots (copias congeladas)  
Muy útil para backups o actualizaciones seguras.  
* Mover datos entre discos sin parar el sistema  
Puedes migrar LVs de un disco a otro en caliente.  

Comandos clave para manejar volúmenes LVM:

**1. Physical Volumes (PV):**
- `pvcreate /dev/sdX1` — crear PV  
- `pvs` — listar PVs  

**2. Volume Groups (VG):**
- `vgcreate miVG /dev/sdX1` — crear VG  
- `vgextend miVG /dev/sdY1` — añadir disco al VG  
- `vgs` — listar VGs  

**3. Logical Volumes (LV):**
- `lvcreate -L 10G -n miLV miVG` — crear LV  
- `lvextend -L +5G /dev/miVG/miLV` — ampliar  
- `lvreduce -L 5G /dev/miVG/miLV` — reducir (¡cuidado!)  
- `lvs` — listar LVs  

**4. Filesystems sobre LVM:**
- `mkfs.ext4 /dev/miVG/miLV` — formatear  
- `mount /dev/miVG/miLV /mnt`  
- `resize2fs /dev/miVG/miLV` — redimensionar FS ext4 tras un lvextend  

### Gestión de Paquetes

#### a) Licencias
Las licencias determinan cómo se puede usar, modificar o distribuir un software.  
Las más comunes en Linux son:

- **GPL (General Public License)**  
  Software libre que obliga a mantener la misma licencia en derivados.

- **LGPL (Lesser GPL)**  
  Similar a GPL, pero permite enlazar librerías con software propietario.

- **MIT**  
  Muy permisiva; permite usar, modificar y redistribuir con pocas restricciones.

- **Apache 2.0**  
  Permite uso comercial y modificaciones, incluye protección de patentes.

- **BSD**  
  Muy permisiva; casi sin restricciones.

- **Propietarias**  
  Código cerrado; uso limitado según fabricante.

---

#### b) Repositorios y gestores de paquetes
Los gestores de paquetes permiten **instalar, actualizar y eliminar** software desde **repositorios** oficiales o de terceros.

**Debian / Ubuntu / derivados (APT):**
- `apt update` — actualiza índice de paquetes  
- `apt upgrade` — actualiza el sistema  
- `apt install <paquete>` — instala  
- `apt remove <paquete>` — elimina  
- `apt search <paquete>` — buscar  
- Configuración: `/etc/apt/sources.list`

**RHEL / CentOS / Fedora (DNF / YUM):**
- `dnf install <paquete>`  
- `dnf remove <paquete>`  
- `dnf update`  
- `dnf search <paquete>`  

**Arch Linux (pacman):**
- `pacman -S <paquete>` — instalar  
- `pacman -R <paquete>` — eliminar  
- `pacman -Syu` — actualizar sistema  

**OpenSUSE (zypper):**
- `zypper install <paquete>`  
- `zypper remove <paquete>`  
- `zypper refresh`  

**Repositorios comunes:**
- Oficiales de la distribución  
- Repositorios de terceros (como PPA en Ubuntu)  
- Repos privados empresariales  
- AUR (Arch User Repository) para Arch
