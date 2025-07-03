---
id: 2025062101
type: permanent
tags:
  - Linux
  - Console
  - español
author: Imran Afzai
---
## Como conectarse a una maquina remota

Principalmente necesitamos una maquina a la queramos conectarnos xD, en esta maquina podemos escribir los siguiente comando para obtener la ip privada del servidor

```bash
ip a
ifconfig
```

Estos comandos nos van a devolver la información de las interfaces de red de nuestra maquina, ip privada, mascara de red, broadcast, ipv6, etc. Lo que nos importa es la ipv4 privada de nuestro server.

Para conectarnos podemos hacerlo de dos formas pero en ambas vamos a necesitar el usuario del server (puede ser root o uno de los que este en el servidor).

Dependiendo de si se tiene la llave publica de nuestro pc en el server o no, nos va pedir password

```bash
# Primera forma
ssh usuarioServer@192.168.191.90

# Segunda forma (y mi favorita)
ssh -l usuarioServer 192.168.191.90

# Luego puedes preguntarte a ti mismo quien eres (linux resolvio esa pregunta hace mucho xd)
whoami
```

---

## Que changos es root?

1. Root account: La cuenta root o el usuario root en una maquina de linux es el ser mas poderoso :v, tiene accesos a todos los comandos y a todos los archivos del sistema
2. Root como /: Es la primera capa de directorios en linux, esta también se llama root (se le conoce en spanish como ruta raíz)
3. la casa del root: El usuario root tiene un directorio en /root el cual es llamado root home directory

---

## Como changos puedo cambiar una password?

- Se debe cambiar una password cuando te logueas por primera vez en un sistema, este principio se usa normalmente cuando los sysadmin te dan un una password básica por primera vez
- Commando:

```bash
passwd <username>
# Le damos la password actual
# Luego la nueva
# Le decimos otra vez la nueva para que no se le olvide xdd
```

---

## Intro a filesystem

**Que es un FileSystem**:
Es un sistema que se usa para operar y manejar archivos. Este sistema controla como se guarda la data o como es recuperada/retornada

- Un sistema operativo guarda archivos y directorios de una forma ordenada y estructurada
	- Archivo de configuración de sistema = Directorio A
	- Archivos del usuario = Directorio B
	- Archivos log = Directorio C
	- Comandos o Scripts = Directorio D ... y así xd

- Existen diferentes tipos de sistemas de archivos. En general, se maneja un mismo sistema de archivos entre distros linux, aunque la distro sea distinta se tiende a manejar un mismo estándar.
- Las mejoras a los sistemas de archivos vienen en nuevos releases de un sistema operativo, cada nuevo sistema de archivos tiene un nombre diferente
	- ext3, ext4, xfs, NFS, FAT, etc


## Estructura del Filesystem y su descripción 

**/boot  --> Contiene el archivo que es usado por el boot loader (grub.cfg)**
**/root  --> Es el home del usuario root. No es lo mismo que /**
**/dev  --> Dispositivos del sistema (disco, cd rom, altavoces, USB flash, teclado, mouse, etc.)**
**/etc  --> Archivos de configuración. Aquí normalmente vamos a encontrar los archivos de configuración de los programas que instalemos**
**/bin ->/usr/bin  --> Comandos del día a día del usuario**
**/sbin ->/usr/sbin  --> Comandos del sistema/sistema de archivos**
**/opt  --> Significa Optional add-on applications (No hace parte de las aplicaciones del sistema operativo)**
**/proc  --> Procesos corriendo (Solo los existentes en memoria)**
**/lib ->usr/lib --> Librerías de archivos escritas en C que son necesarias por comandos y apps**

```bash
strace -e open pwd
```

**/tmp  --> Directorio de archivos temporales**
**/home --> Directorio del usuario**
**/var  --> Logs del sistema**
**/run  --> Los demonios del sistema son los primeros en empezar (como systemd y udev). Este directorio se usa para guardar de manera temporal los archivos de ejecución como los archivos PID**
**/mnt  --> Se usa para "Montar" un sistema de archivos externo (como el NFS)**
**/media  --> Para el montaje de cdroms**

---

## Navegando por el sistema de archivos

Si vamos a navegar por un sistema de archivos UNIX vamos a necesitar estos comanditos

```bash
cd # nos sirve para cambiar de directorio
pwd # nos retorna la ruta actual en la que estamos
ls # Lista los archivos y las carpetas del directorio actual
```


## Propiedades de los archivos y carpetas en Linux

Cada archivo o carpeta en linux tiene propiedades e informacion detallada

| Type        | # of Links | Owner | Group | Size | Month | Day | Time  | Name     |
| ----------- | ---------- | ----- | ----- | ---- | ----- | --- | ----- | -------- |
| drwxr-xr-x. | 21         | root  | root  | 4096 | Feb   | 27  | 13:33 | var      |
| lrwxrwxrwx. | 1          | root  | root  | 7    | Feb   | 27  | 13:15 | bin      |
| -rw-r--r--  | 1          | root  | root  | 0    | Mar   | 2   | 11:15 | testfile |

**TYPE:** Todo lo que inicia con d es un directorio, con l hace referencia a que es un archivo link que lleva a otro archivo y si inicia con -, significa que es simplemente un archivo con texto plano.

**Numero # of links:** Es el numero de de hard links de un archivo, para un directorio es el numero de subdirectorios que tienen mas de un directorio principal y el mismo

**Owner:** usuario que tiene acceso al recurso

**Group:** grupo que tiene acceso al recurso

**Size:** Tamaño del archivo

**Date data:** Información del mes, día, minutos y segundos en el que se creó/editó el archivo

**Name:** Nombre del archivo 


## Filesystem Paths

Existen dos formas de navegar dentro de un sistema de archivos
- Rutas absolutas
- Rutas relativas

Una ruta absoluta siempre empieza con un "/". Este indica que el path inicia desde el directorio raíz. Un ejemplo de ruta absoluta es:
**cd /var/log/samba**

Una ruta relativa no empieza con un "/". Esta identifica la locación relativa de tu posición actual. Un ejemplo de esto es: 

**cd /var   cd log   cd samba**


## Creando Archivos y directorios

```bash
# Crear archivos
touch # Crea un archivo
cp # Copia un archivo
vi # Abre el editor vi y crea un archivo
nano # abre el editor nano y crea un archivo

# Crear directrios
mkdir # Crea un directorio

ls -ltr #Lista todos los arhivos creados en orden del ultimo modificado
```


## Copiando directorios

``` bash
# copiar un directorio

cp -r <source folder> <Destination folder>
```


## Encontrar archivos y directorios

**Locate:**
Locate una prebuild database (es decir un archivo con toda la info de nuestro filesystem), esta sea actualiza regularmente. Mientras que **Find** itera sobre el filesystem para encontrar archivos.
Locate es mucho mas veloz que find, pero, puede fallar si la db no es actualizada (aunque esto lo hace ahora en tiempo casi casi que real)
Si nos llega a generar algún problema la db de locate podemos usar el comando
updatedb.


```bash
# find examples

find . -name "name of directory"

find / -name "netplan"

# locate examples
locate studies
locate /things/python
/home/jenhz/things/python.py
```


## El pinche Joker :v (Wild Cards)

Un wildcard es un caracter que puede ser usado como un sustituto para cualquier clase de caracteres en una búsqueda.

- * - Representa cero o mas caracteres
- ? - representa un solo caracter
- [] - representa un rango de caracteres

```bash
rm -rf /* #moricion
touch abcd{1..9}-xyz # crea nueve files :v
rm *xyz # borra todos los xyz
ls -lah ?bcd* # nos da todos los archivos que tengan bcd en el nombre
ls -lah *[cd]* # muestra todos los archivos que tengan cd en el directorio actual y en los subdirectorios, tambien nos muestra el peso de dichos archivos
```


## Tipos de archivos en Linux

| File Symbol | Meaning                        |
|-------------|--------------------------------|
| -           | Regular file                   |
| d           | Directory                      |
| l           | Link                           |
| c           | Special file or device file    |
| s           | Socket                         |
| p           | Named pipe                     |
| b           | Block device                   |

## Soft and hard links

- **inode:** Puntero o numero de archivo en el disco duro
- **Soft link:** Este Link puede ser removido o si el archivo es eliminado o renombrado
- **Hard link:** Eliminando o moviendo el archivo original no afectaremos el hard ink

NOTA: No podemos crear un soft o hard link en el mismo directorio con el mismo nombre, normalmente esto debemos hacerlo entre directorios. Para esto vamos a crear una prueba en el /tmp.  Los hard link solo trabajan si están en la misma partición, si queremos crear un hard link en /var desde nuestro home pero /var esta en otra partición, no nos va a permitir crear el hard link


```bash
ln # hard link
ln -s # soft link
```

```bash
# prueba con soft link

touch hulk # archivo creado en /home/user/things
cd /tmp
ln -s /home/usr/things
ls -ltr
lrwxrwxrwx  1 root root        23 Jun 21 20:58 hulk -> /home/user/things/hulk

# Cuando escribimos algo en el archivo (ya sea desde el original o el linked) cambia el contenido en ambos

rm /home/user/things/hulk # borra el archivo en la ruta origina y al ser un soft link se borra el enlace entre ambos :(
```

```bash 
# prueba con hard link

touch /home/jenhz/things/hulk
echo "Hulk hard linked" >  /home/jenhz/things/hulk]
cd /tmp
ln /home/jenhz/things/hulk
-rw-r--r--  2 root root        17 Jun 21 21:08 hulk
# Lo demas funciona igual, no veremos que se crea un link ya que aunque compartan lo mismo que escribamos, si borramos el archivo original no se eliminara el archivo linked
```