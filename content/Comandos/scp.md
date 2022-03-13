---
title: "Scp"
date: 2021-12-08T19:56:14+01:00
lastmod: 2021-12-08T19:56:14+01:00
publishdate: 2021-12-08T19:56:14+01:00
description: ""
weight: 10
---

Todos utilizamos de forma asidua el comando scp (Secure Copy) en Linux y Unix para transferir archivos entre hosts a través del protocolo SSH. Normalmente nos ceñimos a su utilización básica pero viene bien conocer algunos trucos o usos poco habituales del comando (para algunos) para mejorar u optimizar su utilización. Ahí van unos cuantos ejemplos que espero os sean de utilidad.

## Uso básico de SCP

Comencemos por lo básico, transferencia de un archivo entre el host local y un host remoto o viceversa y entre hosts remotos.

**Transferencia de un archivo de local a un host remoto:**

* Archivo local: archivo.tar.gz
* Usuario remoto: foo
* Host remoto: 192.168.1.100
* Ruta remota donde enviar el archivo: /home/foo

```bash
scp archivo.tar.gz foo@192.168.1.100:/home/foo/
```


**Transferencia de un archivo de un host remoto a local:**

* Archivo remoto: archivo.tar.gz
* Usuario remoto: foo
* Host remoto: 192.168.1.100
* Ruta local donde guardar el archivo: /home/bar/

```bash
scp foo@192.168.1.100:/home/foo/archivo.tar.gz /home/bar/
```

**Transferencia de archivos entre dos hosts remotos:**

* Host remoto #1: 192.168.1.100
* Archivo remoto en #1: archivo.tar.gz
* Usuario remoto #1: foo
* Host remoto #2: 192.168.1.200
* Ruta remota de #2 donde guardar el archivo: /home/bar/
* Usuario remoto #2: bar

```bash
scp foo@192.168.1.100:/home/foo/archivo.tar.gz bar@192.168.1.200:/home/bar/
```

**Copia recursiva y con wildcards (*)**

Aunque no suele ser lo más habitual usar scp para realizar copias recursivas o de grandes cantidades de archivos (para eso solemos usar rsync en combinación con ssh), conviene saber que es posible hacerlo.

Para copiar recursivamente archivos y directorios utilizamos el parámetro «-r». El siguiente ejemplo muestra como copiar todo el contenido (archivos y directorios) de la ruta local «/home» a la ruta remota «/var/tmp». En la slida estándar van apareciendo los archivos (los directorios no aparecen) transferidos:
```bash
scp -r /home/ foo@192.168.1.100:/var/tmp
foo@192.168.1.100 password:
VBoxSVC.log.1 100% 2460 2.4KB/s 00:00
VirtualBox.xml 100% 2793 2.7KB/s 00:00
VBoxSVC.log.3 100% 3395 3.3KB/s 00:00
solaris.xml 100% 3573 3.5KB/s 00:00
CentOS6.xml 100% 3674 3.6KB/s 00:00
```

Si en lugar de copia recursiva lo que queremos es utilizar wildcards (*) podemos hacerlo como si un «cp» u otro comando estándar se tratara. En el siguiente ejemplo copiamos todos los archivos de /home/foo pero sin recursividad de directorios:

```bash
scp /home/foo/* bar@192.168.1.100:/tmp
```

Podemos especificar extension de archivos o patrones para filtrar:

```bash
scp /home/foo/*.log bar@192.168.1.100:/tmp
```

**Limitar velocidad de SCP**

Para limitar el ancho de banda utilizado durante la transferencia de datos con scp se utiliza el parámetro «-l» seguido del máximo de Kbit/s a utilizar durante la copia.

```bash
 scp -l 500 archivo.tar.gz foo@192.168.1.100:/tmp
```

**Habilitar compresión al tráfico de SCP**

El parámetro -C activa la compresión en la transferencia de datos vía sc. Si al utilizarlo activamos también el debug ampliaremos la información por salida estándar con los ratios de compresión aplicados entre otras estadísticas:

```bash
scp -Cv archivo.tar.gz foo@192.168.1.100:/home/foo/

[...]
Transferred: sent 86744, received 1696 bytes, in 7.0 seconds
Bytes per second: sent 12471.8, received 243.8
debug1: Exit status 0
debug1: compress outgoing: raw data 93447, compressed 84606, factor 0.91
debug1: compress incoming: raw data 148, compressed 116, factor 0.78
```

**Mantener stats de archivos (atime, mtime, permisos)**
Al igual que cuando copiamos con el comando «cp», el parámetro «-p» mantendrá los datos de fecha de modificación, fecha de acceso y modos del fichero original:

```bash
scp -p archivo.tar.gz bar@192.168.1.100:/tmp
```

Ejemplo de stats:

$ stat archivo.tar.gz
  File: `archivo.tar.gz'
  Size: 93178     	Blocks: 184        IO Block: 4096   regular file
Device: 805h/2053d	Inode: 550538      Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/    alex)   Gid: ( 1000/    alex)
Access: 2014-09-20 09:10:14.590601162 +0200
Modify: 2014-09-20 09:10:31.210683568 +0200
Change: 2014-09-20 09:10:31.210683568 +0200
 Birth: -
Cambiar el cifrado a utilizar para la transferencia de datos
No suele ser muy habitual, pero si por algún casual necesitas cambiar el tipo de cifrado a utilizar durante la transferencia de archivos por scp, puedes hacerlo con el parámetro «-c». Algunos métodos de cifrado disponibles (protocol version 2) son «3des-cbc», «aes128-cbc», «aes192-cbc», «aes256-cbc», «aes128-ctr», «aes192-ctr», «aes256-ctr», «arcfour128», «arcfour256», «arcfour», «blowfish-cbc, y «cast128-cbc».

$ scp -c aes128-ctr archivo.tar.gz bar@192.168.1.100:/tmp
Verbose y Quiet Mode
Habrá ocasiones en las que queramos visualizar la mayor información posible de las conexiones scp/ssh y la transferencia de archivos y otras en las que no queramos nada de salida estándar.

Para ampliar la información lo hacemos igual que con ssh, con el parámetro «-v». Básicamente tendremos información en modo debug de la conexión, autenticación y configuración.

$ scp -v archivo.tar.gz foo@192.168.1.100:/home/foo
Executing: program /usr/bin/ssh host 192.168.1.100, user foo, command scp -v -t -- /home/foo
OpenSSH_5.9p1 Debian-5ubuntu1.4, OpenSSL 1.0.1 14 Mar 2012
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 19: Applying options for *
debug1: Connecting to 192.168.1.100 [192.168.1.100] port 22.
debug1: Connection established.
debug1: identity file /home/alex/.ssh/id_rsa type 1
debug1: Checking blacklist file /usr/share/ssh/blacklist.RSA-2048
[...]
Y si lo que queremos es deshabilitar toda la salida estándar, utilizaremos la opción «-q», que deshabilitará tanto la barra de progreso como cualquier aviso o mensaje de diagnóstico.

$ scp -q archivo.tar.gz foo@192.168.1.100:/home/foo
Cambiar el puerto de conexión
Esta es fácil. Si SSH está corriendo en un puerto distinto al TCP 22 deberemos cambiarlo también al utilizar SCP. Para ello utilizamos el parámetro «-P» seguido del puerto:

$ scp -P 6922 archivo.tar.gz foo@192.168.1.100:/home/foo
Utilizar un fichero de configuración de cliente ssh distinto (ssh_config)
Podemos realizar configuraciones personalizadas para SSH y SCP en un fichero de configuración alternativo e invocarlo desde línea de comandos al ejecutar SCP, de este modo nos ahorramos tener que añadir múltiples parámetros que pueden ser configurados en el archivo de configuración. Especificamos el path a ssh_config con el parámetro «-F»:

$ scp -F /home/alex/ssh_config_SCP archivo.tar.gz foo@192.168.1.100:/tmp

Executing: program /usr/bin/ssh host ...
OpenSSH_5.9p1 Debian-5ubuntu1.4, OpenSSL 1.0.1 14 Mar 2012
debug1: Reading configuration data /home/alex/ssh_config_SCP
debug1: /home/alex/ssh_config_SCP line 19: Applying options for *
[...]