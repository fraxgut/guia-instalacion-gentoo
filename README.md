<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a name="readme-top"></a>
<!--
*** Thanks for checking out the Best-README-Template. If you have a suggestion
*** that would make this better, please fork the repo and create a pull request
*** or simply open an issue with the tag "enhancement".
*** Don't forget to give the project a star!
*** Thanks again! Now go create something AMAZING! :D
-->



<!-- ESCUDOS PROYECTO -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->


[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![VPL+ACR License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url]



<!-- LOGO PROYECTO -->
<br />
<div align="center">
  <a href="https://github.com/fraxgut/guia-instalacion-gentoo">
    <img src="images/Logo.png" alt="Logo" width="80" height="80">
  </a>

<h3 align="center">Guía para la instalación de Gentoo GNU Linux</h3>

  <p align="center">
    Variante: AMD64 Hardened MUSL (BTRFS + LUKS + LVM + LTO + LLVM + Zen)
    <br />
    <a href="https://github.com/fraxgut/guia-instalacion-gentoo"><strong>Explorar el repositorio »</strong></a>
    <br />
    <br />
    <a href="https://github.com/fraxgut/guia-instalacion-gentoo/issues">Reportar Errores</a>
    ·
    <a href="https://github.com/fraxgut/guia-instalacion-gentoo/issues">Solicitar Mejoras</a>
  </p>
</div>



<!-- ÍNDICE -->
<details>
  <summary>Índice</summary>
  <ol>
    <li><a href="#sobre-el-proyecto">Sobre el Proyecto</a></li>
    <li><a href="#prerequisitos">Prerequisitos</a></li>
    <li><a href="#instalación">Instalación</a></li>
    <li><a href="#hoja-de-ruta">Hoja de ruta</a></li>
    <li><a href="#contribuir">Contribuir</a></li>
    <li><a href="#licencia">Licencia</a></li>
    <li><a href="#contacto">Contacto</a></li>
    <li><a href="#reconocimientos">Reconocimientos</a></li>
  </ol>
</details>



<!-- SOBRE EL PROYECTO -->
## Sobre el proyecto

Este tutorial en GitHub te guiará paso a paso en la instalación de Gentoo, una distribución de Linux altamente personalizable. En particular, se enfoca en la instalación de Gentoo amd64 con el compilador LLVM (Clang) y Musl, utilizando el kernel Zen y el sistema de archivos Btrfs. Además, se incorporará próximamente información sobre la configuración de SELinux para brindar una mayor seguridad a tu sistema. 

<p align="right">(<a href="#readme-top">ir al inicio</a>)</p>


<!-- PREREQUISITOS -->
## Prerequisitos
#### Descargar un sistema para la instalación
Para la instalación de Gentoo, no es necesario utilizar la imagen oficial del sistema operativo, aunque es una opción válida que se puede descargar desde su portal.
Personalmente, sugiero utilizar SystemRescueCD, ya que personalmente lo considero una herramienta confiable y efectiva para el proceso de instalación. Puedes descargarlo desde el siguiente enlace: https://www.system-rescue.org/Download/.
Antes de iniciar el proceso de instalación de Gentoo, es importante decidir qué medio se utilizará para llevar a cabo la instalación del sistema operativo. Dependiendo de las circunstancias, el medio de instalación puede ser un dispositivo externo, como un CD/DVD o un USB, la imagen del sistema descargada directamente o la conexión remota mediante herramientas como SSH.

#### Definir el medio de instalación
El **primer** caso se utiliza cuando se dispone de un equipo físico en el que se instalará el sistema operativo, es decir, cuando el usuario se encuentra en su casa u oficina, o en alguna situación similar. El **segundo** caso se utiliza cuando se trabaja con una máquina virtual, como VirtualBox, VMWare, QEMU, entre otros. Finalmente, el **tercer** caso se utiliza cuando se trabaja de forma remota, como por ejemplo, con un servidor virtual privado. En cualquiera de estos casos, el **sistema anfitrión** será el equipo desde el que se descargará el sistema operativo, mientras que el **sistema invitado** será el equipo en el que se instalará Gentoo.

##### Grabar la imagen en algún dispositivo (caso 1)
Se recomienda utilizar un dispositivo USB en lugar de un CD/DVD. Las siguientes instrucciones están dirigidas a sistemas UNIX (GNU/Linux, BSD, macOS) con GPT/UEFI. Para grabar la imagen en un dispositivo USB, sigue los siguientes pasos:
1. Renombra la imagen descargada a 'SystemRescueCD.iso'.
2. Abre la terminal (salta al paso 6 si utilizas CD/DVD).
3. Navega al directorio donde se encuentra la imagen (por ejemplo: cd Descargas).
4. Identifica el dispositivo USB que utilizarás para grabar la imagen con el comando lsblk (por ejemplo: /dev/sdc).
5. Formatea el dispositivo USB con los siguientes comandos (reemplazar "DISPOSITIVO"):
	- `sgdisk --zap-all /dev/DISPOSITIVO`
	- `sgdisk --clear --new 1:0:0 --typecode=2:8200 --change-name=1:LiveUSB /dev/DISPOSITIVO`
	- `mkfs.fat -F 32 -n LiveUSB /dev/disk/by-partlabel/LiveUSB`
6. Aplica los siguientes comandos:
	- `isohybrid SystemRescueCD.iso`
	- `dd if=SystemRescueCD.iso of=/dev/DISPOSITIVO bs=8192k; sync`
7. Desconecta el dispositivo USB (Nota: se puede utilizar una herramienta como UUI para automatizar todo este proceso o para MBR/BIOS).

*Nota para usuarios de Windows: Se puede utilizar la herramienta Rufus (https://rufus.ie/es/) para realizar una grabación simple en un dispositivo USB. En caso de utilizar un CD/DVD, se deben utilizar las herramientas del sistema.*
Una vez grabada la imagen en el dispositivo USB, conéctalo al sistema invitado e inicia SystemRescueCD.

##### Utilización de una máquina virtual (caso 2)
El proceso es sencillo: descarga la imagen, ingrésala en tu software de virtualización preferido y, simplemente, inicia el sistema.

##### Conexión remota (caso 3)
Si prefieres no realizar el proceso de instalación tú mismo, comunícate con tu proveedor para que pueda insertar la imagen que necesitas en tu sistema. Él se encargará de realizar el caso 1 o caso 2 por ti.

<p align="right">(<a href="#readme-top">ir al inicio</a>)</p>

<!-- INSTALACIÓN -->
## Instalación

#### Primeros pasos
Los primeros pasos consisten en realizar una conexión a internet exitosa, establecer una contraseña segura para el usuario administrador y asegurar el sistema mediante la configuración de SSH, si es necesario.

SystemRescueCD viene con NetworkManager preinstalado, lo que permite utilizar las herramientas que proporciona este programa. En primer lugar, debes abrir la terminal para comenzar con la instalación, en caso de que el sistema se haya iniciado en un escritorio y no en un terminal virtual. Aquí se realizarán la mayoría de los comandos, en el caso de que no se trabaje con SSH. De todas formas, es fundamental conectarse a internet. Esto es automático en la mayoría de los casos en los que se utiliza una red cableada, no obstante, algunos servidores virtuales requieren una configuración manual.

Para una configuración rápida, utiliza el comando `nmtui`, donde el proceso de configuración debería ser bastante intuitivo. Si es necesario, se pueden especificar manualmente la dirección IP y los servidores DNS.Luego, utiliza el comando `ping 9.9.9.9` para verificar que la conexión a internet haya sido exitosa. 

Es importante cambiar la clave del administrador mediante el comando passwd para evitar conexiones no autorizadas. Se recomienda utilizar un generador de contraseñas o elegir cinco palabras al azar y mezclarlas, siempre utilizando una variación de mayúsculas con minúsculas. También se pueden utilizar números y símbolos para hacer la contraseña más segura. Lo ideal es que la contraseña sea tan robusta como sea posible, siempre y cuando se pueda recordar fácilmente.

Finalmente, si lo deseas, puedes configurar SSH para realizar el proceso de instalación desde otra computadora. Esto es especialmente útil en caso de utilizar una terminal virtual, para poder copiar y pegar los comandos necesarios sin necesidad de introducirlos manualmente. Para esto, utiliza el comando `vim /etc/ssh/sshd_config` (o tu editor de texto preferido, se utilizará vim en esta guía) para editar el documento. Lo interesante aquí es:

- Descomentar el puerto 22 `#Port 22` → `Port 22`
- Descomentar `#ListenAddress` y colocar la dirección IP correspondiente, por ejemplo: `ListenAddress 192.168.5.50`
- Descomentar `#PasswordAuthentication yes` → `PasswordAuthentication yes`

Ahora existen dos alternativas, la primera es cerrar el documento y desactivar IpTables con `systemctl stop iptables`, ya que está en conflicto con SSH. Para mayor seguridad, se pueden realizar configuraciones manuales para filtrar el puerto 22, aunque esto no es necesario en la mayoría de los casos. De todas formas, si se desea, se pueden utilizar los siguientes comandos:

- `LOIF=lo`
- `SSHPORT=22`
- `iptables -A INPUT -i $LOIF -j ACCEPT`
- `iptables -A OUTPUT -o $LOIF -j ACCEPT`
- `iptables -A INPUT -p tcp -m tcp --dport $SSHPORT -j ACCEPT`
- `iptables -A OUTPUT -p tcp --sport $SSHPORT -m state --state ESTABLISHED -j ACCEPT`
- `iptables -P INPUT DROP`
- `iptables -P OUTPUT DROP`

Arregla la fecha con `ntpd -q -g` y luego realiza la conexión a SSH en tu sistema anfitrión con `ssh root@ip`, reemplazando "ip" con tu dirección IP correspondiente.

#### Configuración del disco
A partir de ahora, la guía se centrará en proporcionar los comandos necesarios para realizar la configuración del disco. En primer lugar, es necesario descubrir el nombre del disco donde se instalará el sistema operativo utilizando el comando `lsblk` o `fdisk -l`. Una vez que se ha obtenido el nombre del disco (por ejemplo, /dev/sda), se debe establecer una variable para el disco como `DRIVE=/dev/ndx`, donde ndx es el nombre del disco. Si se interrumpe la instalación por algún motivo, se puede partir desde "[x]".

Se debe realizar una limpieza total del disco utilizando los siguientes comandos:
- `cryptsetup open --type plain $DRIVE container --key-file /dev/urandom`
- `dd if=/dev/urandom of=/dev/mapper/container status=progress bs=1M`
- `cryptsetup close container`
- `sgdisk --zap-all $DRIVE`

Descubre si tu sistema es UEFI o BIOS y procede con los pasos correspondientes:
- `[ -d /sys/firmware/efi ] && echo UEFI || echo BIOS`

A continuación, se debe proceder a crear la partición y encriptarla (donde "NOMBRE" es el nombre del sistema anfitrión y "xxx" es el número del disco):
1. `sgdisk --clear --new=1:0:+1GiB --typecode=1:ef00 --change-name=1:EFI --new=2:0:0 --typecode=2:8300 --change-name=2:NOMBRE_xxx_sys $DRIVE`# Sistemas UEFI
2. `sgdisk --clear --new=1:0:+1MiB --typecode=1:ef02 --change-name=1:GRUB --new=2:0:+1GiB --typecode=2:8300 --change-name=2:BOOT --new=3:0:0 --typecode=3:8300 --change-name=3:NOMBRE_xxx_sys $DRIVE` # Sistemas BOOT
- `cryptsetup --type luks2 --cipher aes-xts-plain64 --hash sha512 --iter-time 5000 --key-size 512 --pbkdf argon2id --use-random --verify-passphrase luksFormat /dev/disk/by-partlabel/NOMBRE_xxx_sys`
- `cryptsetup open /dev/disk/by-partlabel/NOMBRE_xxx_sys root` [x]

Posteriormente, se debe configurar el sistema LVM:
- `pvcreate /dev/mapper/root`
- `vgcreate NOMBRE_vg1 /dev/mapper/root`
- `lvcreate -l +100%FREE NOMBRE_vg1 --name lv1`

Después de crear el sistema LVM, se debe formatear el disco para tener un sistema de archivos:
1. `mkfs.vfat -F 32 -n EFI /dev/disk/by-partlabel/EFI` # Sistemas UEFI
2. `mkfs.ext2 -L BOOT -b 4096 /dev/disk/by-partlabel/BOOT` # Sistemas BOOT
- `mkfs.btrfs --force --label BTRFS /dev/mapper/NOMBRE_vg1-lv1`

Una vez que se ha formateado el disco, se deben establecer los subvolúmenes de BTRFS junto con el montaje:
- `o=defaults,x-mount.mkdir` [x]
- `o_btrfs=$o,commit=120,compress=lzo,rw,space_cache,ssd,noatime,nodev,nosuid` [x]
- `o_boot=$o,nosuid,nodev,noatime,fmask=0022,dmask=0022,codepage=437,iocharset=iso8859-1,shortname=mixed,errors=remount-ro` [x] # Sistemas UEFI
- `o_lboot=$o,nosuid,nodev,noatime,errors=remount-ro` # Sistemas BOOT
- `mkdir -p /mnt/gentoo` [x]
- `mount -t btrfs LABEL=BTRFS /mnt/gentoo`
- `btrfs subvolume create /mnt/gentoo/@`
- `btrfs subvolume create /mnt/gentoo/@home`
- `btrfs subvolume create /mnt/gentoo/@snapshots`
- `umount -R /mnt/gentoo`
- `mount -t btrfs -o $o_btrfs,subvol=@ LABEL=BTRFS /mnt/gentoo`[x]
- `mkdir /mnt/gentoo/home`
- `mkdir /mnt/gentoo/.snapshots`
- `mkdir /mnt/gentoo/tmp`
- `mkdir -p /mnt/gentoo/boot/efi`
- `mkdir -p /mnt/gentoo/var/cache`
- `mount -t btrfs -o $o_btrfs,subvol=@home LABEL=BTRFS /mnt/gentoo/home` [x]
- `mount -t btrfs -o $o_btrfs,subvol=@snapshots LABEL=BTRFS /mnt/gentoo/.snapshots` [x]
1. `mount -o $o_boot LABEL=EFI /mnt/gentoo/boot` [x] # Sistemas UEFI
2. `mount -o $o_lboot LABEL=BOOT /mnt/gentoo/boot` [x] # Sistemas BOOT
- `btrfs subvolume create /mnt/gentoo/var/tmp`
- `btrfs subvolume create /mnt/gentoo/var/swap`
- `btrfs subvolume create /mnt/gentoo/opt`
- `btrfs subvolume create /mnt/gentoo/srv`
- `chmod 1777 /mnt/gentoo/tmp`
- `chmod 1777 /mnt/gentoo/var/tmp`
- `chmod 755 /mnt/gentoo/home`

Para crear el archivo de intercambio "swapfile", es necesario definir previamente el tamaño que se requiere, el cual puede variar según la cantidad de RAM instalada en el sistema. La regla general es utilizar "2 veces la cantidad de RAM", pero también puedes usar la siguiente tabla como referencia:

| Tamaño RAM | Tamaño del "swapfile" (Sin Hibernar) | Tamaño del "swapfile" (Con Hibernar) |
|------------|--------------------------------------|--------------------------------------|
| 256MB      | 256MB                                | 512MB                                |
| 512MB      | 512MB                                | 1GB                                  |
| 1GB        | 1GB                                  | 2GB                                  |
| 2GB        | 1GB                                  | 3GB                                  |
| 3GB        | 2GB                                  | 5GB                                  |
| 4GB        | 2GB                                  | 6GB                                  |
| 6GB        | 2GB                                  | 8GB                                  |
| 8GB        | 3GB                                  | 11GB                                 |
| 12GB       | 3GB                                  | 15GB                                 |
| 16GB       | 4GB                                  | 20GB                                 |
| 24GB       | 5GB                                  | 29GB                                 |
| 32GB       | 6GB                                  | 38GB                                 |
| 64GB       | 8GB                                  | 72GB                                 |
| 128GB      | 11GB                                 | 139GB                                |

Una vez definido el tamaño del archivo, se aplican los siguientes comandos (asegúrate de reemplazar "CANTIDADGB" con el número correspondiente):
- `truncate -s 0 /mnt/gentoo/var/swap/swapfile`
- `chattr +C /mnt/gentoo/var/swap/swapfile`
- `btrfs property set /mnt/gentoo/var/swap/swapfile compression none`
- `chmod 600 /mnt/gentoo/var/swap/swapfile`

Luego, ejecuta **uno** de los siguientes comandos:
1. `dd if=/dev/urandom of=/mnt/gentoo/var/swap/swapfile bs=1G count=CANTIDADGB status=progress`
2. `dd if=/dev/urandom of=/mnt/gentoo/var/swap/swapfile bs=1M count=CANTIDADMB status=progress`

Para activar el archivo de intercambio, ejecuta los siguientes comandos:
- `mkswap -L swapfile /mnt/gentoo/var/swap/swapfile`
- `swapon /mnt/gentoo/var/swap/swapfile` [x]
- `cd /mnt/gentoo` [x]

#### Instalación de Gentoo
##### Descarga de Gentoo
En este paso se determina la versión de Gentoo que se instalará. Si bien esta guía está enfocada en la variante de Hardened MUSL, cualquier versión debería servir. Cabe mencionar que para esta variante, no se establece una zona horaria ni una localización, por lo que no se cubrirán en esta guía. Para comenzar, busca el enlace de la variante que deseas en esta página: https://www.gentoo.org/downloads/. Luego, reemplaza en "GENTOO_LINK" el enlace correspondiente.
- `GENTOO_LINK=https://bouncer.gentoo.org/fetch/root/all/releases/amd64/autobuilds/20230409T163155Z/stage3-amd64-musl-hardened-20230409T163155Z.tar.xz`
- `wget $GENTOO_LINK`

A continuación, asegurémonos de que el archivo sea seguro:
- `wget -O - https://qa-reports.gentoo.org/output/service-keys.gpg | gpg --import`
- `openssl dgst -r -sha512 archivo.tar.xz` *(reemplaza "archivo" con la variante de Gentoo descargada)*
- `openssl dgst -r -whirlpool archivo.tar.xz`

Luego, extrae el archivo:
- `tar xpvf archivo.tar.xz --xattrs-include='*.*' --numeric-owner`
- `rm archivo.tar.xz`

Después, establece los repositorios y traslada la configuración DNS:
- `mkdir --parents /mnt/gentoo/etc/portage/repos.conf`
- `cp /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/repos.conf/gentoo.conf`
- `cp --dereference /etc/resolv.conf /mnt/gentoo/etc/`

Monta algunos directorios adicionales:
- `mount -t proc /proc /mnt/gentoo/proc` [x]
- `mount -R /sys /mnt/gentoo/sys` [x]
- `mount --make-rslave /mnt/gentoo/sys` [x]
- `mount -R /dev /mnt/gentoo/dev` [x]
- `mount --make-rslave /mnt/gentoo/dev` [x]
- `mount -B /run /mnt/gentoo/run` [x]
- `mount --make-slave /mnt/gentoo/run` [x]
- `test -L /dev/shm && rm /dev/shm && mkdir /dev/shm` [x]
- `mount -t tmpfs -o nosuid,nodev,noexec shm /dev/shm` [x]
- `chmod 1777 /dev/shm /run/shm` [x] *(si /run/shm no existe, no es ningún problema)*

Es importante revisar el archivo "make.conf" para configurar algunas variables básicas. Este archivo se puede modificar posteriormente según sea necesario. Por ahora, solo es necesario hacer algunos cambios básicos. Abre el archivo "make.conf" con vim (modifica la variable "NPROC" a gusto):
- `vim /mnt/gentoo/etc/portage/make.conf`

```
# DEFAULT VARIABLES
# Compilation
#COMMON_FLAGS="-O2 -pipe"
#CFLAGS="${COMMON_FLAGS}"
#CXXFLAGS="${COMMON_FLAGS}"
#FCFLAGS="${COMMON_FLAGS}"
#FFLAGS="${COMMON_FLAGS}"
#CHOST="x86_64-gentoo-linux-musl"

# Language
#LC_MESSAGES=C

# CUSTOM VARIABLES
# Compilation
NTHREADS="auto"
NPROC="$(nproc)"
COMMON_FLAGS="-march=native -O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"
CHOST="x86_64-gentoo-linux-musl"
MAKEOPTS="-j${NPROC}"
EMERGE_DEFAULT_OPTS="--jobs ${NPROC} --load-average ${NPROC}"
#PORTAGE_SCHEDULING_POLICY="idle"
PORTAGE_NICENESS="19"
PORTAGE_IONICE_COMMAND="/usr/local/bin/io-priority \${PID}"

# Language
LC_MESSAGES=C
```

Hay que crear el archivo "io-priority" con `vim /mnt/gentoo/usr/local/bin/io-priority`:

```
#!/bin/bash
PID=${1}
ionice -c 3 -p ${PID}
chrt -p -i 0 ${PID}
```

Finalmente, ejecutamos `chmod +x /usr/local/bin/io-priority` para poder ejecutar la secuencia.

##### Entrando a Gentoo y Configuración Básica
Para entrar al sistema operativo Gentoo, ejecuta los siguientes comandos:
- `chroot /mnt/gentoo /bin/bash` [x]
- `source /etc/profile` [x]
- `export PS1="(chroot) ${PS1}"` [x]
- `o=defaults,x-mount.mkdir` [x]
- `o_btrfs=$o,commit=120,compress=lzo,rw,space_cache,ssd,noatime,nodev,nosuid` [x]
- `o_boot=$o,nosuid,nodev,noatime,fmask=0022,dmask=0022,codepage=437,iocharset=iso8859-1,shortname=mixed,errors=remount-ro` [x] # Sistemas UEFI
- `o_lboot=$o,nosuid,nodev,noatime,errors=remount-ro` [x] # Sistemas BOOT
- `DRIVE=/dev/ndx` [x] #reemplazar ndx con el número de disco

Luego, debes sincronizar los paquetes para el gestor "Portage" y establecer el repositorio de musl. Pero primero, debes elegir un servidor y asegurarte de que el perfil correcto esté seleccionado:
- `emerge-webrsync`
- `emerge --sync`
- `eselect profile list`
- `eselect profile set --force "x"` *(reemplaza x con el perfil de Hardened MUSL, o el de tu elección)*
- `emerge app-eselect/eselect-repository dev-vcs/git app-portage/mirrorselect app-portage/cpuid2cpuflags`
- `eselect repository enable musl`
- `mirrorselect -i -o >> /etc/portage/make.conf` *(selecciona los servidores que más te acomoden)*
- `emerge --sync`
- `emerge -uvDN @world`
- `emerge --depclean`
- `emerge app-editors/neovim`

En este punto, se puede realizar una revisión del archivo /etc/portage/make.conf, si se desea. Es importante fijar las banderas USE necesarias para tu dispositivo. El documento que se mostrará a continuación es solo un ejemplo en particular y no debe ser imitado en su totalidad. Para obtener más información, visita la página https://wiki.gentoo.org/wiki//etc/portage/make.conf para posibles cambios. También puedes añadir `ACCEPT_KEYWORDS="~amd64"` si quieres tener un sistema actualizado. A continuación se presentan los comandos para este sistema en particular:
- `echo 'CPU_FLAGS_X86="'$(cpuid2cpuflags | grep -o 'CPU_FLAGS_X86: .*' | paste -sd " " | sed 's/CPU_FLAGS_X86: //')'"' >> /etc/portage/make.conf`
Hay que modificar el archivo `nvim /etc/portage/make.conf`, especialmente revisar las banderas USE:

```
# /etc/portage/make.conf

# DEFAULT VARIABLES                                                 
# Compilation:                                                      
#COMMON_FLAGS="-O2 -pipe"                                           
#CFLAGS="${COMMON_FLAGS}"                                           
#CXXFLAGS="${COMMON_FLAGS}"                                         
#FCFLAGS="${COMMON_FLAGS}"                                          
#FFLAGS="${COMMON_FLAGS}"                                           
#CHOST="x86_64-gentoo-linux-musl"                                   
                                                                    
# Language:                                                         
# #LC_MESSAGES=C                                                    
                                                                    
# CUSTOM VARIABLES                                                  
# Compilation:                                                      
NTHREADS="auto"
NPROC=1
COMMON_FLAGS="-march=native -O2 -pipe"                              
CFLAGS="${COMMON_FLAGS}"                                            
CXXFLAGS="${COMMON_FLAGS}"                                          
FCFLAGS="${COMMON_FLAGS}"                                           
FFLAGS="${COMMON_FLAGS}"                                            
CHOST="x86_64-gentoo-linux-musl"                                    
MAKEOPTS="-j${NPROC}"                                            
EMERGE_DEFAULT_OPTS="--jobs ${NPROC} --load-average ${NPROC}" 
#PORTAGE_SCHEDULING_POLICY="idle"                                  
PORTAGE_NICENESS="19"                                              
PORTAGE_IONICE_COMMAND="/usr/local/bin/io-priority \${PID}"        
                                                                   
# Language:                                                        
L10N="es-CL es-ES es en-US en"                                     
LC_MESSAGES=C                                                      
LINGUAS="es-CL es-ES es en-US en"                                  
                                                                   
# USE Flags:                                                       
DISABLE="-alsa -gnome -kde -pipewire -pulseaudio -qt -qt5 -qt6 -X" 
ENABLE="argon2 btrfs clang coreboot default-compiler-rt default-libcxx default-lld device-mapper fpm llvm llvm-libunwind lto lvm lz4 lzo mount nginx networkmanager pgo polly qemu readline srvdir truetype vim-syntax zsh zsh-completions"                                  
USE="${DISABLE} ${ENABLE}"                                         
                                                                   
# Features:
ACCEPT_KEYWORDS="" # Gentoo Stable
# ACCEPT_KEYWORDS="~amd64" # Gentoo Unstable
ACCEPT_LICENSE="-* @FREE"                                          
CPU_FLAGS_X86="mmx mmxext sse sse2 sse3"                           
FEATURES="buildpkg"                                                
GRUB_PLATFORMS="efi-64"                                            
INPUT_DEVICES=""                                                   
MICROCODE_SIGNATURES="-S"                                          
VIDEO_CARDS="qxl"

# Mirrors:
GENTOO_MIRRORS="https://mirror.ufro.cl/gentoo/ http://mirror.ufro.cl/gentoo/ rsync://gentoo.ufro.cl/gentoo/"

# vim:ft=gentoo-make-conf
```

Como se está trabajando en un sistema MUSL hay que hacer algunos arreglos para que funcione correctamente la localización y la zona horaria:
- `eselect repository add 12101111-overlay git https://github.com/12101111/overlay.git` # repositorio que contiene musl-locales
- `eselect repository enable guru` # repositorio GURU
- `emerge --sync`
- `rm -rf /etc/portage/package.use /etc/portage/package.accept_keywords /etc/portage/package.license /etc/portage/package.mask` 
- `echo "sys-apps/musl-locales ~amd64" >> /etc/portage/package.accept_keywords`
- `emerge sys-apps/musl-locales emerge sys-libs/timezone-data net-misc/ntp  app-arch/lz4 dev-libs/lzo sys-fs/btrfs-progs sys-fs/cryptsetup sys-fs/lvm2 sys-apps/kbd sys-apps/sed sys-apps/grep app-crypt/gnupg dev-libs/openssl app-shells/zsh app-shells/zsh-completions app-shells/gentoo-zsh-completions`
- `TZ="Country/City"` *(reemplaza "Country/City" con los datos correspondientes)*
- `export TZ="Country/City"` *(fija la variable TZ al horario que se desee `ls /usr/share/zoneinfo`)*
- `echo TZ="Country/City" >> /etc/env.d/00musl` 
- `locale -a` *(elige la localización preferida, en este caso español)*
- `export MUSL_LOCPATH="/usr/share/i18n/locales/musl"`
- `export CHARSET="es_ES.UTF-8"`
- `export LANG="es_ES.UTF-8"`
- `export LC_COLLATE="es_ES.UTF-8"`
- `echo MUSL_LOCPATH="/usr/share/i18n/locales/musl" >> /etc/env.d/00musl`
- `echo CHARSET="es_ES.UTF-8" >> /etc/env.d/00musl` 
- `echo LANG="es_ES.UTF-8" >> /etc/env.d/00musl` 
- `echo LC_COLLATE="es_ES.UTF-8" >> /etc/env.d/00musl` 
- `eselect locale list`
- `eselect locale set "x"` *(reemplaza x con el número correspondiente)*
- `ntpd -dnq -p pool.ntp.org`
- `hwclock -u -w`
- `env-update && source /etc/profile && export PS1="(chroot) ${PS1}"`

##### Compilando el Kernel
Primero, se puede instalar el paquete "linux-firmware" o omitir si no se quiere trabajar con código que no sea completamente libre. No obstante, para la mayoría de los sistemas, es recomendable instalarlo. Se utilizará el kernel "zen-sources" en esta guía.

- `echo "sys-kernel/linux-firmware @BINARY-REDISTRIBUTABLE" >> /etc/portage/package.license`
- `echo "sys-kernel/zen-sources ~amd64" >> /etc/portage/package.accept_keywords`
- `emerge sys-kernel/linux-firmware sys-kernel/zen-sources sys-kernel/dracut`
- `eselect kernel list`
- `eselect kernel set "x"` *(cambia x por el kernel que necesitas)*
- `cd /usr/src/linux`

La compilación del kernel es una de las áreas más complejas. Mi recomendación es darse un tiempo para aprender sobre los componentes de tu sistema, investigar a fondo lo más posible, tomar nota y activar o desactivar los componentes innecesarios. De todas formas, dejaré una recomendación sobre componentes que deberían ser posibles de activar/desactivar en la mayoría de los sistemas. Asegúrate de saber cuántos núcleos/hilos tienes con `lscpu`.

```
General setup  --->
    [ ] POSIX Message Queues
    [ ] Enable process_vm_readv/writev syscalls
    [ ] uselib syscall
    [*] Auditing support
    Kernel compression mode --->
	(*) LZ4
    Timers subsystem  --->
	Timer tick handling --->
	    (*) Periodic timer ticks (constant rate, no dynticks)
	[ ] Old Idle dynticks config
        <*> High Resolution Timer Support
    CPU/Task time and stats accounting --->
	[ ] BSD Process Accounting
	[ ] Export task/process statistics through netlink
    (15) Kernel log buffer size
    (15) CPU kernel log buffer size contribution
    (13) Temporary per-CPU printk log buffer size
    [*] Initial RAM filesystem and RAM disk (initramfs/initrd) support
    Compiler optimization level --->
    	(*) Optimize more for performance (-O3)
		
Processor type and features  --->
  [*] Symmetric multi-processing support
  [ ] Enable MPS table
  [ ] Support for extended (non-PC) x86 platforms
  [*] Linux guest support --->
      [*] Enable Paravirtualization code
      [*] KVM Guest support (including kvmclock)
  Processor family (AMD-Opteron/Athlon64)  --->
     (v) AMD X # selecciona si es AMD (reemplaza X por tu modelo)
     (v) Intel X # selecciona si es Intel (reemplaza X por tu modelo)
     (v) Intel-Native optimizations autodetected by the compiler # selecciona si es Intel y no conoces tu modelo
     (v) AMD-Native optimizations autodetected by the compiler # selecciona si es AMD/QEMU y no conoces tu modelo
  (v) Maximum number of CPUs # Reemplaza con la cantidad de hilos que tiene tu CPU
  [ ] Reroute for broken boot IRQs
  [*] Machine Check / overheating reporting 
  	[v] Intel MCE Features # selecciona si el procesador es Intel
  	[v] AMD MCE Features # selecciona si el procesador es AMD
  [ ] IOPERM and IOPL Emulation
  [ ] Enable 5-level page tables support
  [ ] Check for low memory corruption
  [*] MTRR cleanup support
  	(1) MTRR cleanup enable value
	(1) MTRR cleanup spare reg value
  [*] EFI runtime service support 
  [*] EFI stub support
  [*] EFI mixed-mode support
  [ ] kexec system call
  [ ] kernel crash dumps
  
Power management and ACPI options  --->
    -*- CPU Frequency Scaling
        Default CPUFreq governor (userspace) --->
	    (*) userspace
	-*- 'performance' governor
	<*> 'powersave' governor
	<*> 'userspace' governor for userspace frequency scaling
	<*> 'ondemand' cpufreq policy governor
	<*> 'conservative' cpufreq policy governor
	
Binary Emulations --->
   [*] IA32 Emulation
   
[*] Virtualization --->
	<*> Kernel-based Virtual Machine (KVM) support
	<M>   KVM for Intel processors support
	<M>   KVM for AMD processors support
	
[*] Enable loadable module support
	[*] Module unlodaing
		[*] Forced module unloading
		
-*- Enable the block layer --->
		[*] Block layer bio throttling support
		[*] Block layer debugging information in debugfs
    	Partition Types --->
    		[*] Advanced partition selection
			[*] PC BIOS (MSDOS partition tables) support
    		[*] EFI GUID Partition support
		IO Schedulers --->
			< > MQ deadline I/O scheduler
			< > Kyber I/O scheduler
			<*> BFQ I/O scheduler
				[*] BFQ hierarchical scheduling support
				
[*] Networking support  ---> # cambia los ajustes según tu sistema
		Networking options  --->
    		<*> The IPv6 protocol
			<*> Packet socket
   		<*> 802.1d Ethernet Bridging
 	 	[*] Wireless  --->
        	<*>   cfg80211 - wireless configuration API
        	[*]     cfg80211 wireless extensions compatibility
		
Device Drivers ---> # cambia los ajustes según tu sistema
	[*] Virtio drivers  --->
        	<*> PCI driver for virtio devices
	[*] Staging drivers  --->
    [v] Enable modesetting on radeon by default # para GPU de AMD
	Generic Driver Options --->
    		[*] Maintain a devtmpfs filesystem to mount at /dev
    		[*] Automount devtmpfs at /dev, after the kernel mounted the rootfs
  	SCSI device support  ---> 
    		<*> SCSI device support
    		<*> SCSI disk support
		<*> SCSI CDROM support
		[*] Asynchronous SCSI scanning
		<*> Virtual (SCSI) Host Bus Adapter
		[*] SCSI low-level drivers  --->
            	[*] virtio-scsi support
  	<*> Serial ATA and Parallel ATA drivers (libata)  --->
    		[*] ATA ACPI Support
    		[*] SATA Port Multiplier support
    		<*> AHCI SATA support (ahci)
    		[*] ATA BMDMA support
    		[*] ATA SFF support (for legacy IDE and PATA)
    		<*> Intel ESB, ICH, PIIX3, PIIX4 PATA/SATA support (ata_piix)
  	NVME Support --->
    		<*> NVM Express block device
    		[*] NVMe multipath support
    		[*] NVMe hardware monitoring
    		<M> NVM Express over Fabrics FC host driver
    		<M> NVM Express over Fabrics TCP host driver
    		<M> NVMe Target support
    		[*] NVMe Target Passthrough support
    		<M> NVMe loopback device support
    		<M> NVMe over Fabrics FC target driver
    		< > NVMe over Fabrics FC Transport Loopback Test driver (NEW)
    		<M> NVMe over Fabrics TCP target support
  	Network device support --->
	    [*] Network core driver support
    	    <*> PPP (point-to-point protocol) support
    	    	<*> PPP over Ethernet
    	    	<*> PPP support for async serial ports
    	    	<*> PPP support for sync tty ports
    	    <*> Universal TUN/TAP device driver support
            <*> Virtio network driver
  	HID support  --->
    	    -*- HID bus support
    	    <*> Generic HID driver
    	    [*] Battery level reporting for HID devices
    	    USB HID support  --->
        	    <*> USB HID transport layer
  	[*] USB support  --->
    	<*> xHCI HCD (USB 3.0) support
    	<*> EHCI HCD (USB 2.0) support
    	<*> OHCI HCD (USB 1.1) support
  	<*> Unified support for USB4 and Thunderbolt  --->
	Firmware Drivers  --->
    	EFI (Extensible Firmware Interface) Support  --->
        	<*> EFI Variable Support via sysfs
 	Graphics support  --->
    	Frame buffer Devices  --->
        	<*> Support for frame buffer devices  --->
            	[*] EFI-based Framebuffer Support
    	[ ] Bootup logo  --->
    	<v> Direct Rendering Manager (XFree86 4.1.0 and higher DRI support)  ---> # para GPU de Intel
         	<v> Intel 8xx/9xx/G3x/G4x/HD Graphics # para GPU de Intel
         	[v] Enable modesetting on intel by default # para GPU de Intel
		<*> Intel 8xx/9xx/G3x/G4x/HD Graphics # para GPu de Intel
        	[*] Enable Intel GVT-g graphics virtualization host support # para GPU de Intel
        <*> Enable KVM host support Intel GVT-g graphics virtualization # para GPU de Intel
	 	<v> Nouveau (nVidia) cards # para GPU de Nvidia
		<*> Virtio GPU driver
    [*] Multiple devices driver support (RAID and LVM) --->
        <*> Device mapper support
        <*> Crypt target support
		<*> Thin provisoning support
	<*> Mirror target
	<*> Multipath target
    	<*> I/O Path Selector based on the number of in-flight I/Os
    	<*> I/O Path Selector based on the service time 
    [*] Block Devices ---> 
        <*> Loopback device support
		(0) Number of loop devices to pre-create at init time
		<*> Virtio block driver
	<*> VFIO Non-Privileged userspace driver framework
            <*> Mediated device driver framework
	Character devices ---> 
    <*> Hardware Random Number Generator Core support --->
    	<*> VirtIO Random Number Generator support
		
File systems  --->
    <*> Second extended fs support
    <*> The Extended 3 (ext3) filesystem
    <*> The Extended 4 (ext4) filesystem
    <*> Btrfs filesystem support
    <*> FUSE (Filesystem in Userspace) support
  DOS/FAT/NT Filesystems  --->
    <*> MSDOS fs support
    <*> VFAT (Windows-95) fs support
  Pseudo Filesystems --->
    [*] /proc file system support
    [*] Tmpfs virtual memory file system support (former shm fs)
	
-*- Cryptographic API --->
    Block ciphers --->	
		<*> AES cipher algorithms
    	<*> AES cipher algorithms (x86_64)
		<*> Serpent cipher algorithm 
    	<*> Twofish cipher algorithm
	Length-preserving ciphers and modes --->
	    <*> LRW support 
		<*> XTS support
	Hashes, digests and MACs --->
    	<*> RIPEMD-160 digest algorithm 
		<*> SHA224 and SHA256 digest algorithm
    	<*> SHA384 and SHA512 digest algorithms 
    	<*> Whirlpool digest algorithms
	Compression --->
		<*> LZ4
		<*> LZO
	Userspace interface --->
		<*> Hash algorithms
    	<*> Symmetric key cipher algorithms

Kernel hacking --->
	RCU Debugging --->
		(3) RCU CPU stall timeout in seconds

Gentoo Linux --->
  Generic Driver Options --->
    [*] Gentoo Linux support
    [*] Linux dynamic and persistent device naming (userspace devfs) support
    [*] Select options required by Portage features
        Support for init systems, system and service managers  --->
          [*] OpenRC, runit and other script based systems and managers
          [ ] systemd
```

Para modificar el archivo "dracut.conf", debes ejecutar el comando `nvim /etc/dracut.conf` y agregar o modificar las siguientes líneas:

```
# Include only modules from the system:
hostonly="yes"

# Compression:
compress="lz4"

# Add modules:
add_dracutmodules+=" btrfs crypt crypt-gpg crypt-loop dm fs-lib i18n img-lib lvm resume selinux uefi-lib "
```

Luego, para compilar el kernel y crear la imagen de inicio, debes ejecutar los siguientes comandos:
- `make -j$(nproc) && make -j$(nproc) modules_install && make -j$(nproc) install`
- `dracut -H --force --kver $(cat /usr/src/linux/include/config/kernel.release)`
                                                                                                                                 
##### Configuración final (pre-reinicio)
Ahora queda la configuración final, principalmente, montar los dispostivos al inicio, fijar el nombre para el sistema, colocar una contraseña segura y establecer una conexión a internet. Personalmente uso NetworkManager por su TUI, pero hay opciones más minimalistas si se desea (como netirfc). 

Primero, hay que crear el archivo fstab:
1. `UEFI_UUID=$(blkid -s UUID -o value /dev/disk/by-partlabel/EFI)`
2. `BOOT_UUID=$(blkid -s UUID -o value /dev/disk/by-partlabel/BOOT)`
- `LUKS_UUID=$(blkid -s UUID -o value /dev/disk/by-partlabel/NOMBRE_xxx_sys)`
- `ROOT_UUID=$(blkid -s UUID -o value /dev/mapper/NOMBRE_vg1-lv1)`
- `SWAP_UUID=$(findmnt -no UUID -T /var/swap/swapfile)`
- `RESUME=$(btrfs inspect-internal map-swapfile -r /var/swap/swapfile)` # para GRUB
- `echo "luks-$LUKS_UUID UUID=$LUKS_UUID none luks" >> /etc/crypttab`
- `cat <<EOF > /etc/fstab`:

```
/dev/mapper/luks-$LUKS_UUID	/	btrfs	$o_btrfs,subvol=@	0	1
UUID=$UEFI_UUID	/boot	vfat	$o_boot	0	2
UUID=$BOOT_UUID	/boot	ext2	$o_lboot	0	2
/dev/mapper/NOMBRE_vg1-lv1	/home	btrfs	$o_btrfs,subvol=@home	0	2
/dev/mapper/NOMBRE_vg1-lv1	/.snapshots	btrfs	$o_btrfs,subvol=@snapshots	0	2
tmpfs	/tmp	tmpfs	defaults,nosuid,nodev	0	0
/var/swap/swapfile none swap sw 0 0
EOF
```

Luego, se debe fijar un nombre para el sistema y una contraseña segura:
- `echo NOMBRE > /etc/hostname`
- `chsh -s /bin/zsh && exec zsh` # Mi recomendación es ocupar zsh, y es seguro hacer el cambio en este punto
- `passwd` # Recomiendo seguir las mismas directrices de antes
- `useradd -m -G users,wheel,audio,cdrom,floppy,portage,usb,video -s /bin/zsh nombreusuario` # *(reemplaza nombreusuario con el usuario que ocuparás)*
- `passwd nombreusuario`

Hay que instalar NetworkManager mediante la fijación de la bandera USE "networkmanager" en `/etc/portage/make.conf` y ejecutar los siguientes comandos:
- `echo "net-wireless/wpa_supplicant dbus" >> /etc/portage/package.use`
- `emerge -uvDN @world`
- `emerge net-misc/networkmanager`
- `rc-update add NetworkManager default`

También es un buen momento para instalar otros programas necesarios:
- `echo "sys-process/snooze ~amd64" >> /etc/portage/package.accept_keywords`
- `emerge app-admin/metalog sys-process/snooze sys-apps/mlocate net-misc/chrony`
- `rc-update add metalog default && rc-update add sshd default && rc-update add chronyd default && rc-update add lvm boot && rc-update add dmcrypt boot`

Finalmente se debe configurar GRUB:
- `echo "sys-fs/cryptsetup argon2 -static-libs" >> /etc/portage/package.use`
- `mkdir -p /etc/portage/patches/sys-boot/grub-2.06`
- `cd /etc/portage/patches/sys-boot/grub-2.06`
- `curl -O https://leo3418.github.io/res/collections/gentoo-config-luks2-grub-systemd/4500-grub-2.06-runtime-memregion-alloc.patch`
- `curl -O https://leo3418.github.io/res/collections/gentoo-config-luks2-grub-systemd/5000-grub-2.06-luks2-argon2-v4.patch`
- `curl -O https://leo3418.github.io/res/collections/gentoo-config-luks2-grub-systemd/9500-grub-AUR-improved-luks2.patch && cd`
- `mkdir -p /etc/portage/env/sys-boot`
- `echo 'GRUB_AUTOGEN=1' >> /etc/portage/env/sys-boot/grub-2.06`
- `emerge sys-boot/grub sys-boot/os-prober dev-libs/libisoburn sys-fs/mdadm`
- `emerge -uvDN @world`
- `mount -o remount,rw,nosuid,nodev,noexec --types efivarfs efivarfs /sys/firmware/efi/efivars` # Solo si es necesario
1. `grub-install --target=x86_64-efi --efi-directory=/boot --recheck --bootloader-id="GRUB"` # Si esto falla, hay que verificar que el sistema esté en UEFI o probar otro sistema, como rEFInd
2. `grub-install --target=i386-pc --boot-directory=/boot --recheck --bootloader-id="GRUB" $DRIVE` # Para sistemas sin UEFI *(reemplazar ndx con tu disco)*

Hay que editar la configuración de GRUB (precisamente "GRUB_CMDLINE_LINUX_DEFAULT") mediante `nvim /etc/default/grub` *(puede añadirse splash para el uso de Plymouth)*:

```
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet dolvm rd.luks.uuid=luks-$LUKS_UUID rd.lvm.lv=NOMBRE_vg1/lv1 cryptdevice=UUID=$LUKS_UUID root=/dev/mapper/NOMBRE_vg1-lv1 rootfstype=btrfs resume=UUID=$SWAP_UUID resume_offset=$RESUME selinux=0"
GRUB_ENABLE_CRYPTODISK=y
```

Con eso, se puede terminar la configuración y reiniciar el sistema para verificar que todo esté funcionando correctamente:
- `grub-mkconfig -o /boot/grub/grub.cfg`
- `reboot`

#### Solución de problemas
Ahora el sistema debería iniciar correctamente en GRUB. Si no funciona, hay que cambiarlo por LILO o rEFInd. En el caso de que no funcione después de introducir la contraseña LUKS, hay que modificar el archivo "fstab" y la configuración de GRUB "GRUB_CMDLINE_LINUX_DEFAULT". Es común que alguno de estos no funcione correctamente dependiendo de tu sistema. También aprovecha de establecer una conexión a internet con `nmtui` o el gestor de redes que hayas instalado.


#### LLVM/Clang
Hay que ejecutar los siguientes comandos:
- `echo "sys-libs/llvm-libunwind static-libs" >> /etc/portage/package.use`
- `echo "sys-libs/libcxx static-libs" >> /etc/portage/package.use`
- `echo "sys-libs/libcxxabi static-libs" >> /etc/portage/package.use`
- `emerge sys-devel/clang sys-devel/llvm sys-libs/compiler-rt sys-libs/llvm-libunwind sys-devel/lld sys-libs/libcxx sys-libs/libcxxabi`
- `eselect repository add toolchain-clang git https://github.com/2b57/toolchain-clang.git`
- `emaint sync -r toolchain-clang`
- `cd /var/db/repos/toolchain-clang`
- `git clone https://github.com/gentoo/releng.git && cd`
- `nvim /etc/portage/repos.conf/clang-musl.conf`

```
[clang-musl]
sync-uri = https://github.com/clang-musl-overlay/clang-musl-overlay.git
sync-type = git
location = /var/db/repos/clang-musl
sync-depth = 1
```

Luego:
- `emerge --sync`
- `echo "LLVM=1 LLVM_IAS=1" >> /etc/portage/env/kernel-clang && mkdir /etc/portage/package.env`
- `echo "sys-kernel/* kernel-clang" >> /etc/portage/package.env/custom`
- `echo "sys-devel/llvm *FLAGS-="-fipa-pta"" >> /etc/portage/package.cflags/ltoworkarounds.conf`
- `eselect profile set --force "x"` # *(reemplazar x con el número correspondiente a clang-musl/hardened)*
- `emerge -c && emerge sys-devel/llvm-conf`
- `emerge --keep-going sys-devel/clang sys-devel/llvm sys-libs/compiler-rt sys-libs/llvm-libunwind sys-devel/lld`

Hay que hacer unos cambios, `nvim /etc/portage/make.conf`:

```                                                                                             
# Compilation:                                                                               
NTHREADS="auto"                                                                              
NPROC="1"                                                                                    
#source /etc/portage/make.conf.lto.defines                                                   
COMMON_FLAGS="-march=native -O2 -pipe"                                                       
#CFLAGS="-march=native ${CFLAGS} -pipe"                                                      
CFLAGS="${COMMON_FLAGS}"                                                                     
CXXFLAGS="${CFLAGS}"                                                                         
FCFLAGS="${COMMON_FLAGS}"                                                                    
FFLAGS="${COMMON_FLAGS}"                                                                     
LDFLAGS="${LDFLAGS} -fuse-ld=lld -rtlib=compiler-rt -unwindlib=libunwind -Wl,--as-needed"    
CHOST="x86_64-gentoo-linux-musl"                                                             
MAKEOPTS="-j${NPROC}"                                                                        
EMERGE_DEFAULT_OPTS="--jobs ${NPROC} --load-average ${NPROC}"                                
#PORTAGE_SCHEDULING_POLICY="idle"                                                            
PORTAGE_NICENESS="19"                                                                        
PORTAGE_IONICE_COMMAND="/usr/local/bin/io-priority \${PID}"                                  
```

- `source /etc/profile && env-update && source .zshrc`
- `llvm-conf --enable-native-links --enable-clang-wrappers --enable-binutils-wrappers llvm-x` # Reemplazar "x" con la versión correspondiente
- `emerge -1euDN @world`

Luego, para compilar el kernel con LLVM, se deben ejecutar los siguientes comandos:
- `cd /usr/src/linux`
- `LLVM=1 LLVM_IAS=1 make -j$(nproc) && LLVM=1 LLVM_IAS=1  make -j$(nproc) modules_install && LLVM=1 LLVM_IAS=1 make -j$(nproc) install`
- `dracut -H --force --kver $(cat /usr/src/linux/include/config/kernel.release)`
- `grub-mkconfig -o /boot/grub/grub.cfg`
- 
Si un archivo falla en instalar revisar acá para parches (https://github.com/clang-musl-overlay/gentoo-patchset).

#### Activando GentooLTO
Ahora se convertirá el sistema a LTO, para eso, hay que ejecutar los siguientes comandos:
- `eselect repository enable mv`
- `eselect repository enable lto-overlay`
- `emaint sync -a`
- `echo sys-config/ltoize ~amd64`
- `echo app-portage/lto-rebuild ~amd64`
- `I_KNOW_WHAT_I_AM_DOING=y emerge sys-config/ltoize app-portage/lto-rebuild`

Hay que hacer unos cambios, `nvim /etc/portage/make.conf`:

```
# Compilation:                                                                               
NTHREADS="auto"                                                                              
NPROC="1"                                                                                    
source /etc/portage/make.conf.lto.defines                                                   
#COMMON_FLAGS="-march=native -O2 -pipe"                                                       
CFLAGS="-march=native ${CFLAGS} -pipe"                                                      
#CFLAGS="${COMMON_FLAGS}"                                                                     
CXXFLAGS="${CFLAGS}"                                                                         
#FCFLAGS="${COMMON_FLAGS}"                                                                    
#FFLAGS="${COMMON_FLAGS}"                                                                     
LDFLAGS="${LDFLAGS} -fuse-ld=lld -rtlib=compiler-rt -unwindlib=libunwind -Wl,--as-needed"    
CHOST="x86_64-gentoo-linux-musl"                                                             
MAKEOPTS="-j${NPROC}"                                                                        
EMERGE_DEFAULT_OPTS="--jobs ${NPROC} --load-average ${NPROC}"                                
#PORTAGE_SCHEDULING_POLICY="idle"                                                            
PORTAGE_NICENESS="19"                                                                        
PORTAGE_IONICE_COMMAND="/usr/local/bin/io-priority \${PID}"
```

Finalmente hay que ejecutar los siguientes comandos:
- `lto-rebuild -r && emerge -e --keep-going @world`


#### Conclusión
Ahora deberías tener un sistema Gentoo Hardened LLVM amd64 con musl y kernel zen, completamente optimizado.

<p align="right">(<a href="#readme-top">ir al inicio</a>)</p>

<!-- HOJA DE RUTA -->
## Hoja de Ruta

- [X] Gentoo GNU Linux AMD64
    - [X] BTRFS
    - [X] LVM
    - [X] Musl
    - [X] LLVM/Clang
    - [X] Zen kernel
    - [X] LTO
    - [ ] SELinux

Consulte la [discusión](https://github.com/github_username/fraxgut/guia-instalacion-gentoo) para ver la lista completa de funciones propuestas (y problemas conocidos).

<p align="right">(<a href="#readme-top">ir al inicio</a>)</p>

<!-- CONTRIBUIR -->
## Contribuir

Las contribuciones son lo que hace que la comunidad de código abierto sea un lugar increíble para aprender, inspirar y crear. Cualquier contribución que hagas será **muy apreciada**.

Si tienes alguna sugerencia para mejorar esto, por favor haz un fork del repositorio y crea un pull request. También puedes abrir una incidencia con la etiqueta "enhancement".
No olvides darle una estrella al proyecto. Gracias de nuevo.

<p align="right">(<a href="#readme-top">ir al inicio</a>)</p>



<!-- LICENCIA -->
## Licencia

Distribuido bajo la licencia VPL+ACR. Consulte `LICENCE` para más información.

<p align="right">(<a href="#readme-top">ir al inicio</a>)</p>



<!-- CONTACTO -->
## Contacto

Franco Gutiérrez - [@fraxgut](https://twitter.com/fraxgut) - contacto@fraxgut.net

Proyecto: [https://github.com/fraxgut/guia-instalacion-gentoo](https://github.com/fraxgut/guia-instalacion-gentoo)

<p align="right">(<a href="#readme-top">ir al inicio</a>)</p>



<!-- RECONOCIMIENTOS -->
## Reconocimientos

* [Guía de Instalación Gentoo](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation)
* [Proyecto MUSL](https://wiki.gentoo.org/wiki/Project:Musl)
* [Proyecto GURU](https://wiki.gentoo.org/wiki/Project:GURU)
* [gentooLTO](https://github.com/InBetweenNames/gentooLTO)
* [toolchain-clang](https://github.com/2b57/toolchain-clang)
* [clang-musl-overlay](https://github.com/clang-musl-overlay/clang-musl-overlay)

<p align="right">(<a href="#readme-top">ir al inicio</a>)</p>



<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/fraxgut/guia-instalacion-gentoo.svg?style=for-the-badge
[contributors-url]: https://github.com/fraxgut/guia-instalacion-gentoo/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/fraxgut/guia-instalacion-gentoo.svg?style=for-the-badge
[forks-url]: https://github.com/fraxgut/guia-instalacion-gentoo/network/members
[stars-shield]: https://img.shields.io/github/stars/fraxgut/guia-instalacion-gentoo.svg?style=for-the-badge
[stars-url]: https://github.com/fraxgut/guia-instalacion-gentoo/stargazers
[issues-shield]: https://img.shields.io/github/issues/fraxgut/guia-instalacion-gentoo.svg?style=for-the-badge
[issues-url]: https://github.com/fraxgut/guia-instalacion-gentoo/issues
[license-shield]: https://img.shields.io/badge/LICENCE-VPL%2BACR-%2362A98B?style=for-the-badge
[license-url]: https://github.com/fraxgut/guia-instalacion-gentoo/blob/main/LICENCE
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/fraxgut
[product-screenshot]: images/screenshot.png
[Next.js]: https://img.shields.io/badge/next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white
[Next-url]: https://nextjs.org/
[React.js]: https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB
[React-url]: https://reactjs.org/
[Vue.js]: https://img.shields.io/badge/Vue.js-35495E?style=for-the-badge&logo=vuedotjs&logoColor=4FC08D
[Vue-url]: https://vuejs.org/
[Angular.io]: https://img.shields.io/badge/Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white
[Angular-url]: https://angular.io/
[Svelte.dev]: https://img.shields.io/badge/Svelte-4A4A55?style=for-the-badge&logo=svelte&logoColor=FF3E00
[Svelte-url]: https://svelte.dev/
[Laravel.com]: https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white
[Laravel-url]: https://laravel.com
[Bootstrap.com]: https://img.shields.io/badge/Bootstrap-563D7C?style=for-the-badge&logo=bootstrap&logoColor=white
[Bootstrap-url]: https://getbootstrap.com
[JQuery.com]: https://img.shields.io/badge/jQuery-0769AD?style=for-the-badge&logo=jquery&logoColor=white
[JQuery-url]: https://jquery.com 


