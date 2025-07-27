---
id: 2025070202
type: permanent
tags:
  - Linux
  - Console
  - español
author: Imran Afzai
---
## Sintaxis de comandos

Los comandos en linux tienen la siguiente sintaxis:
```txt
command option(s) argument(s)
```

**Opciones**
- Modifica la forma en la que el comando trabaja
- Usualmente consiste en un dash (-) y una letra (se les conoce también como banderas)
- Algunos comandos admiten multiples opciones que van separados unos de otros con un dash

**Argumentos**
- La mayoria de los comandos se usan junto a uno o mas argumentos
- Algunos comandos asumen por defecto un argumento si no les pasamos ninguno
- Los argumentos son opcionales para algunos comandos y para otros no

```bash
# Options example
ls -lth 
ls -la

# Arguments examples
ls -l Pictures/
rm -rf Downloads/
```

---
## Files and Directory Permissions (chmod)

### File permissions

Unix es un sistema multiusuario. Cada archivo o directorio puede ser protegido o ser accesible por ciertos usuarios cambiando los permisos de acceso. Cada usuario tiene la responsabilidad del acceso al control de sus archivos.

Los permisos para un archivo o un directorio pueden tener las siguientes restricciones.

- r - read
- w - write
- x - execute = running a program

Cada permiso (rwx) puede ser controlado en tres niveles:

- u - user= yourself
- g - group= pueden ser personas en el mismo proyecto
- o - other= todos en el sistema

Un permiso de archivo o Directorio puede ser mostrado corriendo el comando ls -l
- -rwxrwxrwx

Comando para cambiar permisos
- chmod

```bash
# Quitar permisos de escritura al grupo
chmod g-w file.sh

# Quitar los perimos de lectura a todos
chmod a-r file.sh

# Quitar permiso a user
chmod u-w file.sh

# Quitar permiso de ejecucion a todos
chmod a-x file.sh

# Agregar permisos de lectura y escritura a user
chmod u+rw file.sh

# Agregar permisos de lectura y escritura a group
chmod g+rw file.sh

# Agregar permisos de lecura a otros
chmod o+r file.sh

# Agrgar permisos de ejecucion a todos
chmod a+x file.sh

```

---

## File permissions using numeric mode

Los permisos de un archivo o un directorio pueden ser asignados de manera numerica

```bash
chmod ugo+r FILE
#or
chmod 444 File
# -r--r--r--
```

| Number | Permission Type        | Symbol |
| ------ | ---------------------- | ------ |
| 0      | No Permission          | ---    |
| 1      | Execute                | --x    |
| 2      | Write                  | -w-    |
| 3      | Execute + Write        | -wx    |
| 4      | Read                   | r--    |
| 5      | Read + Execute         | r-x    |
| 6      | Read + Write           | rw-    |
| 7      | Read + Write + Execute | rwx    |

(user)-r--(group)r--(others)r--

---

## File Ownership Commands (chown, chgrp)

Existen dos propietarios de un archivo o directorio, los usuarios y los grupos
Comandos para cambiar la ownership
- **chown** cambia la ownership de un archivo
- **chgrp** cambia la propiedad de grupo de un archivo

Con la opción **-R** podemos aplicar el cambio de manera recursiva (cascada)

---
## Access control List

El ACl es un mecanismo que nos permite de manera mas flexible manejar los permisos de nuestro sistema de archivos. ACL nos permite dar permisos a cualquier usuario o grupo en cualquier recurso del disco.

Un caso de uso muy particular es cuando queremos que un usuario que no es parte de nuestro grupo tenga permisos de lectura o escritura pero, para hacerlo necesitamos darle los permisos sin hacerlo participe de nuestro grupo, aquí es donde ACL nos ayuda a hacer este proceso.

Los comandos que nos permiten asignar o quitar permisos de ACL son:
**setfacl** y **getfacl**.

```bash
# Darle permisos a un usuario
setfacl -m u:user:rwx /path/file
# Darle permisos a un grupo
setfacl -m g:group:rw /path/file
# Para permitir que todos los archivos o directorios hereden las entradas de ACl del directorio en el que se encuentran
setfacl -Rm "entry" /path/dir

# Remover una entrada especifica (para un usuario especifico)
setfacl -x u:user /path/file 
# Remover todas las entradas (para todos los usuarios)
setfacl -b path/file

#Obtener los datos de acl
getfacl /path/file
```

Cuando agregamos un permiso ACL a un archivo o directorio, este va a agregar un signo + al final del permiso. Al configurar un permiso w con ACL no tienes permitido borrar el archivo.

```bash
root@jenhz:/tmp# getfacl tx
# file: tx
# owner: root
# group: root
user::rw-
group::r--
other::r--

root@jenhz:/tmp# setfacl -m u:jenhz:rw /tmp/tx
root@jenhz:/tmp# getfacl tx
# file: tx
# owner: root
# group: root
user::rw-
user:jenhz:rw-
group::r--
mask::rw-
other::r--

root@jenhz:/tmp# ll
total 56
drwxrwxrwt  14 root root 4096 Jul 16 19:59 ./
drwxr-xr-x  23 root root 4096 Jul  2 21:02 ../
drwxrwxrwt   2 root root 4096 Jul 16 19:23 .font-unix/
drwxrwxrwt   2 root root 4096 Jul 16 19:23 .ICE-unix/
drwx------   2 root root 4096 Jul 16 19:23 snap-private-tmp/
drwx------   3 root root 4096 Jul 16 19:45 systemd-private-ba6108012ad442ea83fd1e21885063a7-fwupd.service-LHZk6O/
drwx------   3 root root 4096 Jul 16 19:23 systemd-private-ba6108012ad442ea83fd1e21885063a7-ModemManager.service-2gSP5t/
drwx------   3 root root 4096 Jul 16 19:23 systemd-private-ba6108012ad442ea83fd1e21885063a7-polkit.service-tGRXVk/
drwx------   3 root root 4096 Jul 16 19:23 systemd-private-ba6108012ad442ea83fd1e21885063a7-systemd-logind.service-6xWDvF/
drwx------   3 root root 4096 Jul 16 19:23 systemd-private-ba6108012ad442ea83fd1e21885063a7-systemd-resolved.service-skVWKo/
drwx------   3 root root 4096 Jul 16 19:23 systemd-private-ba6108012ad442ea83fd1e21885063a7-systemd-timesyncd.service-EgHJke/
drwx------   3 root root 4096 Jul 16 19:45 systemd-private-ba6108012ad442ea83fd1e21885063a7-upower.service-atiqVh/
-rw-rw-r--+  1 root root    0 Jul 16 19:56 tx
drwxrwxrwt   2 root root 4096 Jul 16 19:23 .X11-unix/
drwxrwxrwt   2 root root 4096 Jul 16 19:23 .XIM-unix/
```

---

## Comando Tee

Podemos enviar el output de un comando a un archivo con ayuda del comando tee

```bash
ls -lah | tee output
```

---

## Comando AWK

`awk` es un lenguaje de programación orientado a procesar texto línea por línea, campo por campo. Su sintaxis básica es:

```bash
awk 'pattern { action }' filename
```

## Conceptos clave

- **$0**: Toda la línea de texto.
    
- **$1, $2, ...**: Campos individuales (palabras o columnas), separados por espacios por defecto.
    
- **FS**: Field Separator (se puede definir con `-F` o dentro del script).
    
- **NR**: Número de línea actual.
    
- **NF**: Número de campos en la línea actual.

## Ejemplos útiles

1. Mostrar una columna específica

```bash
awk '{ print $1 }' archivo.txt
# Muestra solo la primera columna de cada línea.
```

2. Sumar valores de una columna

```bash
awk '{ sum += $2 } END { print sum }' archivo.txt
#Suma los valores de la segunda columna y los imprime al final.
```

3. Usar separador personalizado (ej. CSV)

```bash
awk -F ',' '{ print $1, $3 }' archivo.csv
# Imprime la primera y tercera columna de un archivo CSV.
```

4. Filtrar líneas por valor

```bash
awk '$3 > 50 { print $1, $3 }' archivo.txt
# Imprime la primera y tercera columna solo si la tercera columna es mayor a 50.
```

5. Contar líneas

```bash
awk 'END { print NR }' archivo.txt
# Muestra la cantidad total de líneas.
```

6. Eliminar líneas vacías

```bash
awk 'NF > 0' archivo.txt
# Muestra solo las líneas que no están vacías.
```

7. Combinar condición y acción

```bash
awk '$1 == "error" { print $0 }' log.txt
# Imprime líneas que empiezan con "error".
```

Tip: Ejecutar comandos directamente

```bash
echo "1 2 3" | awk '{ print $1 + $2 + $3 }'
# Resultado: `6`
```