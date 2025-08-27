---
id: 2025072501
tags:
  - Linux
  - Console
  - español
author: Imran Afzai
type: permanent
---
## Introducción al Comando sed

El comando `sed` es un comando de sustitución que reemplaza o modifica texto, o manipula tu archivo. Es un comando muy poderoso.

## ¿Qué Puede Hacer el Comando sed?

- Reemplazar una cadena en un archivo por otra. Una cadena puede ser una palabra, una oración o cualquier texto dentro del archivo que desees cambiar.
    
- Buscar y eliminar una línea. Por ejemplo, si una línea tiene "A, B, C" y deseas eliminarla, puedes usar `sed` para hacerlo.
    
- Eliminar líneas vacías de un archivo.
    
- Eliminar la primera línea o varias líneas de un archivo.
    
- Reemplazar tabulaciones por espacios.
    

Estos son solo algunos ejemplos de lo que `sed` puede hacer.

## ¿Por Qué Usar sed en Lugar de Editar Manualmente?

Aunque podrías editar archivos manualmente con editores como `vi`, esto no es eficiente para archivos grandes. Por ejemplo, si tienes un archivo con un millón de líneas y mil de ellas contienen una cadena específica, reemplazarlas o eliminarlas manualmente no es práctico. El comando `sed` permite manipular texto en un archivo de forma eficiente.

---

## Preparación de un Archivo de Ejemplo

Supongamos que tienes un archivo llamado `seinfeld-characters` en tu directorio personal. Si no lo tienes, puedes crearlo con una lista de nombres o usar cualquier otro archivo para practicar.

---

## Reemplazar Texto con sed

Para reemplazar una palabra en un archivo, por ejemplo, cambiar "Kenny" por "Lenny":

```bash
sed 's/Kenny/Lenny/g' seinfeld-characters
```

La `g` al final significa global, es decir, reemplazará todas las ocurrencias en el archivo. Por defecto, este comando solo muestra los cambios en pantalla, sin modificar el archivo.

Para que el cambio sea permanente en el archivo, usa la opción `-i`:
```bash
sed -i 's/Kenny/Lenny/g' seinfeld-characters
```

## Eliminar una Palabra del Archivo

Para eliminar una palabra como "Costanza":
```bash
sed 's/Costanza//g' seinfeld-characters
```

De nuevo, esto solo muestra el cambio. Usa `-i` si quieres modificar el archivo.

---

## Eliminar Líneas que Contienen una Cadena

Para eliminar todas las líneas que contienen la palabra "Seinfeld":
```bash
sed '/Seinfeld/d' seinfeld-characters
```

El comando `d` elimina las líneas que coinciden con el patrón.

---

## Eliminar Líneas Vacías

Para eliminar líneas vacías:
```bash
sed '/^$/d' seinfeld-characters
```

Este comando elimina cualquier línea que comience y termine sin contenido (una línea vacía).

---

## Eliminar Líneas Específicas por Número

Eliminar la primera línea:
```bash
sed '1d' seinfeld-characters
```

Eliminar las dos primeras líneas:
```bash
sed '1,2d' seinfeld-characters
```

---
## Reemplazar Tabulaciones por Espacios

Si tu archivo tiene tabulaciones y deseas reemplazarlas por espacios:

```bash
sed -i 's/\t/ /g' seinfeld-characters
```

Para hacer lo contrario (espacios por tabulaciones):
```bash
sed -i 's/ /\t/g' seinfeld-characters
```

---

## Mostrar Rangos de Líneas Específicos

Para mostrar solo las líneas 12 a 18:
```bash
sed -n '12,18p' seinfeld-characters
```

Para mostrar todo excepto las líneas 12 a 18:
```bash
sed '12,18d' seinfeld-characters
```

---

## Agregar una Línea Vacía Después de Cada Línea

Para insertar una línea vacía después de cada línea del archivo:
```bash
sed 'G' seinfeld-characters
```

---

## Sustitución Condicional Excluyendo una Línea

Para reemplazar "Seinfeld" por "S" en todas partes, excepto en la línea 8
```bash
sed '8!s/Seinfeld/S/g' seinfeld-characters
```

---

## Sustitución Desde el Editor vi

Dentro del editor `vi`, puedes sustituir texto con el siguiente comando:
```bash
:s/Seinfeld/Peter/g
```

Este comando reemplaza "Seinfeld" por "Peter" en la línea actual. Para hacerlo en todo el archivo:
```bash
:%s/Seinfeld/Peter/g
```

---

## Aprendizaje Adicional

El comando `sed` es muy poderoso si lo conoces a fondo. Prueba diferentes opciones, busca más ejemplos en línea y practica para dominar su uso.

---

## Puntos Clave

- `sed` es una herramienta poderosa para manipular texto en archivos, permitiendo sustituciones, eliminaciones y cambios de formato.
    
- Usar la opción `-i` aplica los cambios directamente al archivo; sin ella, los cambios solo se muestran en pantalla.
    
- `sed` permite eliminar o reemplazar palabras específicas, líneas, líneas vacías, tabulaciones y espacios.
    
- También permite seleccionar o excluir rangos de líneas específicos y realizar sustituciones dentro del editor `vi`.

---

## Introducción a la Gestión de Cuentas de Usuario

En esta lección aprenderemos cómo gestionar cuentas de usuario en Linux. Hay algunos comandos que se utilizan para gestionar estas cuentas.

---

## Comandos Esenciales para la Gestión de Usuarios y Grupos

- `useradd`: Crea un nuevo usuario
    
- `groupadd`: Crea un nuevo grupo
    
- `userdel`: Elimina un usuario ya creado
    
- `groupdel`: Elimina un grupo
    
- `usermod`: Modifica un usuario
    

Cada vez que se crea un nuevo usuario, su información se guarda en tres archivos diferentes:

- `/etc/passwd`
    
- `/etc/group`
    
- `/etc/shadow`
    

**Nota:** `/etc/passwd` no es la palabra completa "password", solo se llama _passwd_.

---

## Ejemplo: Crear un Usuario con Parámetros

Para ejecutar el comando `useradd` con todos sus parámetros, la sintaxis es la siguiente:
```bash
useradd -G <grupo> -s <shell> -c <descripción> -m -d <directorio_home> <nombre_usuario>
```

Dónde:

- `-G` añade el usuario a un grupo
    
- `-s` especifica el shell
    
- `-c` define la descripción del usuario
    
- `-m` crea el directorio home
    
- `-d` indica la ruta del directorio home
    
- `<nombre_usuario>` es el nombre del usuario
    

---

## Práctica con Comandos de Usuario y Grupo

Primero, confirma tu usuario y directorio actual. Luego, limpia la pantalla y crea un usuario llamado `spiderman`. Antes de crear al usuario, añade un grupo llamado `superheroes`. Para usar estos comandos debes ser **root**:

```bash
su -
useradd spiderman
```

Si el prompt regresa sin error, el usuario `spiderman` ha sido creado. Para verificar:

```bash
id spiderman
```

También puedes verificarlo revisando el directorio home. Notarás que se creó el directorio `/home/spiderman`.

---

## Crear y Verificar un Grupo

Para crear un nuevo grupo llamado `superheroes`:

```bash
groupadd superheroes
```

Para verificar que se creó el grupo:
```bash
cat /etc/group
```

Al final del archivo verás el grupo `superheroes`.

---

## Eliminar un Usuario y su Directorio Home

Para eliminar al usuario `spiderman`:
```bash
userdel spiderman
```

Para eliminar también su directorio home:
```bash
userdel -r spiderman
```

Verifica si el directorio `/home/spiderman` ha sido eliminado. Ya no debería existir.

---

## Eliminar un Grupo

Primero crea un grupo de prueba:
```bash
groupadd nonewgroup
```

Verifica que existe:
```bash
cat /etc/group
```

Para eliminar el grupo:
```bash
groupdel nonewgroup
```

Verifica nuevamente que haya desaparecido del archivo `/etc/group`.

---

## Modificar un Usuario con `usermod`

El comando `usermod` se utiliza para modificar una cuenta de usuario. Puedes ver sus opciones con:
```bash
man usermod
```

Algunas opciones comunes:

- `-a` agrega un usuario a un grupo suplementario
    
- `-c` cambia la descripción del usuario
    
- `-d` cambia el directorio home
    

---

## Añadir un Usuario a un Grupo

Recrea el usuario `spiderman` si fue eliminado:
```bash
useradd spiderman
```

Para añadirlo al grupo `superheroes`:
```bash
usermod -g superheroes spiderman
```

```bash
grep spiderman /etc/group
```

Esto mostrará que `spiderman` forma parte del grupo `superheroes` y probablemente tenga también su grupo propio.

---

## Cambiar la Pertenencia de Grupo de Archivos

Al listar los archivos con `ls -ltr`, puedes notar que el grupo aún es `spiderman`. Para cambiarlo a `superheroes`:
```bash
chgrp -R superheroes /home/spiderman
```

Esto cambia recursivamente el grupo a `superheroes`.

---

## Archivos del Sistema para Información de Usuarios y Grupos

### `/etc/passwd`

Contiene:

- Nombre de usuario
    
- Contraseña cifrada (representada por `x`)
    
- ID de usuario (UID)
    
- ID de grupo (GID)
    
- Descripción
    
- Directorio home
    
- Shell
    

### `/etc/group`

Contiene:

- Nombre del grupo
    
- Contraseña del grupo
    
- ID del grupo (GID)
    
- Lista de usuarios en el grupo
    

### `/etc/shadow`

Contiene solo contraseñas de usuarios (encriptadas). También incluye información como:

- Fecha de expiración de contraseña
    
- Restricciones de longitud
    
- Días antes del vencimiento para advertencias
    

Puedes consultar más con:
```bash
man shadow
```

---

## Filtrar Información de un Usuario

Para obtener la información de un usuario específico como `spiderman` desde `/etc/passwd`:
```bash
grep spiderman /etc/passwd
```

---

## Ejemplo Completo de Creación de Usuario

En entornos corporativos se suelen usar comandos más detallados:
```bash
useradd -G superheroes -s /bin/bash -c "Personaje de Superhéroes" -m -d /home/ironman ironman
```

Para verificar:
```bash
id ironman
```

También puedes verlo en `/etc/passwd`.

---

## Establecer Contraseña a un Usuario

Después de crear un usuario, asegúrate de asignar una contraseña:
```bash
passwd ironman
```

Si falla el chequeo de diccionario, puedes ignorarlo como root. Si no eres root, debes usar una contraseña más segura.

---

## Conclusión

Esto concluye la lección sobre gestión de cuentas de usuario. Los comandos principales son:

- `useradd`
    
- `groupadd`
    
- `usermod`
    
- `userdel`
    
- `groupdel`
    

Los archivos más importantes son:

- `/etc/passwd`
    
- `/etc/group`
    
- `/etc/shadow`
    

---

## Puntos Clave

- Aprendiste a gestionar cuentas de usuario en Linux con los comandos `useradd`, `groupadd`, `usermod`, `userdel` y `groupdel`.
    
- La información de usuarios y grupos se almacena en los archivos `/etc/passwd`, `/etc/group` y `/etc/shadow`.
    
- Se demostró cómo crear, modificar y eliminar usuarios y grupos, y cómo verificar estas operaciones.
    
- Se explicó la estructura y función de los archivos del sistema relacionados con la gestión de usuarios y grupos.

---

## Introducción al Envejecimiento de Contraseñas y Valores Predeterminados de Usuario

El comando `chage` se utiliza por usuario para gestionar el envejecimiento de contraseñas (_password aging_). Este comando te permite especificar parámetros como:

- Días mínimos entre cambios de contraseña
    
- Días máximos de validez de la contraseña
    
- Último día de cambio
    
- Periodo de inactividad
    
- Fecha de expiración
    
- Días de advertencia antes del vencimiento
    

Para establecer valores predeterminados para **todos los usuarios nuevos** que se creen en el sistema, puedes utilizar un archivo de configuración. Esto es útil si necesitas crear múltiples usuarios y deseas aplicar configuraciones coherentes automáticamente.

---

## El Archivo /etc/login.defs

Existe un archivo llamado `/etc/login.defs`. En él encontrarás parámetros como:

- `PASS_MAX_DAYS` (días máximos de validez de la contraseña)
    
- `PASS_MIN_DAYS` (días mínimos entre cambios de contraseña)
    
- `PASS_WARN_AGE` (días de advertencia antes de la expiración)
    
- Longitud mínima de la contraseña
    

Cuando visualizas este archivo, notarás que las líneas que comienzan con el símbolo de número (`#`) son **comentarios**. Este archivo controla el envejecimiento de contraseñas. Si realizas cambios aquí y defines configuraciones predeterminadas según los requisitos de tu empresa, esos ajustes se aplicarán a todas las cuentas nuevas que se creen.

---

### Otros Valores Importantes en /etc/login.defs

- **UID_MIN y UID_MAX**: Definen el rango de identificadores de usuario (UID).  
    Por defecto, los UID comienzan desde 1000. En entornos corporativos, a veces se configura un rango más alto (como desde 10,000 o 20,000) por motivos de seguridad.
    
- **GID_MIN y GID_MAX**: Igual que con los UID, pero para grupos.
    
- **UMASK**: Define los permisos predeterminados al crear archivos o directorios nuevos.
    
- **CREATE_HOME**: Determina si se debe crear el directorio personal automáticamente.
    
- **ENCRYPT_METHOD**: Define el método de cifrado de contraseñas, como `SHA512`.
    

Este archivo es muy importante porque controla cómo se aplica el envejecimiento de contraseñas para cada usuario. Es esencial conocerlo, especialmente en entornos empresariales donde la seguridad debe aplicarse a nivel de usuario.

---

## Uso del Comando chage

El comando `chage` se utiliza para establecer parámetros de envejecimiento de contraseñas para usuarios individuales. **No** se usa para cambiar detalles generales de la cuenta de usuario (para eso se usa `usermod`).

### Opciones Comunes de chage:

- `-d`: Fecha del último cambio de contraseña
    
- `-m`: Días mínimos entre cambios de contraseña
    
- `-M`: Días máximos de validez
    
- `-W`: Días de advertencia antes del vencimiento
    
- `-I`: Días de inactividad después de vencimiento
    
- `-E`: Fecha de expiración de la cuenta
    

Antes de hacer cambios, puedes revisar los valores actuales en el archivo `/etc/shadow` usando `grep`:

```bash
grep nombre_usuario /etc/shadow
```

---

## Ejemplo: Cambiar el Envejecimiento de Contraseña para un Usuario

Supongamos que deseas establecer los siguientes valores para el usuario `babubutt`:

- Mínimo de días entre cambios: 5
    
- Máximo de días de validez: 90
    
- Advertencia antes de vencimiento: 10 días
    
- Inactividad después del vencimiento: 3 días
    

Usa el siguiente comando:
```bash
chage -m 5 -M 90 -W 10 -I 3 babubutt
```

Después de ejecutar el comando, puedes verificar los cambios en `/etc/shadow`.

Los valores para los días mínimos, máximos, advertencia e inactividad se actualizarán según lo especificado.

---

## Aplicar Cambios a Nivel Global

Si deseas aplicar estos cambios **a todos los usuarios nuevos**, debes modificar el archivo `/etc/login.defs`.

Para cambios en usuarios específicos, utiliza el comando `chage`.

---

## Puntos Clave

- El comando `chage` se usa por usuario para establecer parámetros de envejecimiento de contraseña como días mínimos, máximos, advertencias, inactividad y fecha de expiración.
    
- Los valores predeterminados para el envejecimiento de contraseñas y la creación de usuarios se configuran en el archivo `/etc/login.defs`, y se aplican a todos los nuevos usuarios.
    
- Este archivo también define los rangos de UID y GID, la creación automática del directorio home, el método de cifrado de contraseñas y el `UMASK` para nuevos usuarios.
    
- El uso de `chage` permite a los administradores aplicar políticas de contraseñas más estrictas por usuario, reforzando la seguridad del sistema.

---

## Introducción a Cambiar de Usuario y al Acceso Sudo

Esta lección explica cómo cambiar de usuario en Linux y cómo otorgar acceso **sudo** a los usuarios. El acceso sudo permite que un usuario común pueda ejecutar comandos a nivel de superusuario (root).

---

## Cambiar de Usuario con `su`

Para cambiar de un usuario a otro, utiliza el siguiente comando:
```bash
su - nombre_de_usuario
```

Se te pedirá la contraseña del usuario al que estás intentando cambiar. Una vez ingresada correctamente, pasarás a ser ese usuario.

Para cambiar a **root**, usa:
```bash
su -
```

Cuando cambias desde root a otro usuario, **no se te pedirá la contraseña**, ya que la cuenta root tiene los máximos privilegios.

---

## Usar `sudo` para Ejecutar Comandos como Root

El comando `sudo` permite a los usuarios ejecutar comandos con privilegios de superusuario **sin cambiar de cuenta**. Por ejemplo, comandos como `dmidecode` o `fdisk` requieren privilegios de root:
```bash
sudo dmidecode
sudo fdisk
```

Si ejecutas estos comandos sin `sudo`, obtendrás un error de permiso denegado. Para ejecutarlos correctamente sin convertirte en root, debes tener acceso sudo habilitado.

---

## Editar el Archivo sudoers con `visudo`

Solo el usuario root puede otorgar acceso sudo. Para hacerlo de manera segura, se utiliza el comando `visudo`, que abre el archivo `/etc/sudoers`:
```bash
visudo
```

Dentro del archivo sudoers, busca el grupo llamado `wheel`. Este grupo normalmente está configurado para permitir que sus miembros ejecuten **todos los comandos con sudo**.

---

## Otorgar Acceso Sudo Añadiendo un Usuario al Grupo wheel

Para otorgar privilegios sudo a un usuario, agrégalo al grupo `wheel` usando el siguiente comando como root:
```bash
usermod -G wheel nombre_de_usuario
```

Para verificar que el usuario pertenece al grupo `wheel`:
```bash
grep wheel /etc/group
```

Ahora que el usuario es parte del grupo `wheel`, puede ejecutar comandos con `sudo`. La primera vez que use `sudo`, se le pedirá **su propia contraseña**.

---

Cambiar de usuario y otorgar acceso sudo son tareas administrativas esenciales en Linux. El comando `su` permite cambiar entre cuentas de usuario, mientras que `sudo` permite ejecutar comandos privilegiados sin necesidad de ser root. El comando `visudo` se utiliza para editar de forma segura los permisos sudo, y al añadir usuarios al grupo `wheel`, se les concede acceso sudo.

---

## Puntos Clave

- El comando `su` permite cambiar entre cuentas de usuario, incluida la cuenta root, siempre que se proporcionen las credenciales adecuadas.
    
- El comando `sudo` permite a usuarios comunes ejecutar comandos de nivel root sin necesidad de cambiar de cuenta, siempre que tengan permisos asignados.
    
- El comando `visudo` se utiliza para editar de forma segura el archivo `/etc/sudoers`, que controla el acceso sudo de usuarios y grupos.
    
- Añadir un usuario al grupo `wheel` le otorga la capacidad de ejecutar todos los comandos con privilegios sudo.


---

## Monitorear Usuarios en Linux

Monitorear a los usuarios es una responsabilidad fundamental para cualquier administrador de sistemas, ingeniero o profesional de TI. Es esencial observar qué están haciendo los usuarios dentro del sistema.

Algunos de los comandos básicos para monitorear usuarios en Linux son: `who`, `last`, `w`, `finger` e `id`. Existen muchos más, pero estos son los más utilizados en entornos Linux.

Vamos a revisar estos comandos en una consola de Linux para practicar.

---

## El Comando `who`

El comando `who` te muestra cuántas personas están conectadas, su nombre de usuario, el ID de terminal (TTY) y la hora en que iniciaron sesión.

Por ejemplo, si abres varias terminales, cada una será listada por `who` con su terminal correspondiente y la hora de inicio de sesión.

Si inicias sesión como otro usuario (por ejemplo, `spiderman`) y luego ejecutas `who`, verás una entrada adicional para ese usuario.  
Esto demuestra la función principal del comando: mostrar todos los usuarios conectados actualmente al sistema.

**Útil en casos de alta carga del sistema**, para ver cuántas personas están conectadas.

---

## El Comando `last`

El comando `last` proporciona detalles sobre **todos los usuarios que han iniciado sesión** desde el comienzo (registro histórico).

Al ejecutar `last`, se muestran rápidamente todos los registros de inicio de sesión.

Para ver la salida página por página, usa `more` con un pipe:
```bash
last | more
```

La salida muestra el **nombre de usuario**, el **momento del inicio de sesión** y si el usuario aún está conectado.

`last` también muestra **reinicios, apagados y cierres inesperados del sistema**.

Si el sistema se reinicia, apaga o bloquea inesperadamente y sospechas que alguien estaba conectado, puedes usar `last` para ver cuándo inició sesión, desde qué IP y la fecha y hora.

Para ver **solo la primera columna** de la salida (los nombres de usuario):
```bash
last | awk '{print $1}'
```

Para eliminar usuarios duplicados y ver una lista única:

```bash
last | awk '{print $1}' | sort | uniq
```

Esto mostrará solo los nombres de usuarios únicos que han iniciado sesión, como `iafzal`, `reboot`, `spiderman`, entre otros.

---

## El Comando `w`

El comando `w` funciona de forma similar a `who` pero proporciona **más información**:

- Hora de inicio de sesión
    
- Tiempo de inactividad (idle)
    
- Procesos que está ejecutando cada usuario
    

Es una forma más detallada de monitoreo en tiempo real de usuarios activos.

---

## El Comando `finger` _(opcional)_

El comando `finger` proporciona información detallada sobre los usuarios, como el origen de la conexión y el protocolo usado.  
**No está instalado por defecto**, por lo tanto, no lo usaremos por ahora.

---

## El Comando `id`

Cuando ejecutas `id` **sin especificar usuario**, te muestra información sobre tu **propia cuenta**, incluyendo:

- Nombre de usuario
    
- UID (User ID)
    
- GID (Group ID)
    
- Grupos a los que perteneces
    

Si ejecutas `id` con un nombre de usuario, por ejemplo:

```bash
id spiderman
```

Te mostrará la misma información pero para ese usuario específico.

---

## Recomendación de Práctica

Practica estos comandos con diferentes opciones.  
Puedes ver las opciones disponibles usando el comando `man`. Por ejemplo:
```bash
man who
```

Se recomienda ampliamente probar con diferentes opciones para familiarizarte con ellos.

---

## Puntos Clave

- **`who`** muestra quién está conectado al sistema, su nombre de usuario, ID de terminal y hora de inicio de sesión.
    
- **`last`** muestra todos los inicios de sesión desde el principio, incluyendo fechas, horas e IPs.
    
- **`w`** da información más detallada: procesos activos, tiempo de conexión e inactividad por usuario.
    
- **`id`** muestra el UID, GID y grupos a los que pertenece un usuario, ya sea el actual o uno especificado.


---

## Comunicarse con los Usuarios en un Sistema Linux

Esta lección trata sobre cómo comunicarte con los usuarios que están conectados a un sistema Linux, especialmente cuando se requiere realizar **mantenimiento del sistema** o gestionar recursos.

---

## Identificar Usuarios Conectados

Los usuarios conectados al sistema podrían estar ejecutando aplicaciones o bases de datos.  
Es importante identificarlos, especialmente si necesitas **apagar el sistema** para mantenimiento o si alguna aplicación está generando carga excesiva.

Para saber quiénes están conectados:
```bash
users
```

Este comando muestra una lista con los nombres de usuario de las sesiones activas.

---

## Enviar Mensajes a Todos los Usuarios

El comando `wall` se utiliza para **enviar un mensaje a todos los usuarios conectados** al sistema.  
Es muy útil cuando necesitas notificar un apagado, reinicio o mantenimiento del sistema.
```bash
wall
```

Después de escribir `wall` y presionar Enter, escribe tu mensaje. Ejemplo:
```bash
Por favor, cierre su sesión. El sistema se apagará por mantenimiento.
Imran.
```

Cuando termines, presiona **Ctrl + D** para enviar el mensaje.  
El mensaje aparecerá en las terminales de todos los usuarios conectados.

---

## Enviar Mensajes a un Usuario Específico

Para enviar un mensaje a un solo usuario, utiliza el comando `write` seguido del nombre del usuario:
```bash
write spider
```

Después del comando, escribe tu mensaje. Ejemplo:

```bash
Hola, spider, tu red se está expandiendo demasiado.
Por favor, deja de esparcir tu red.
```

Presiona **Enter** para enviar cada línea.  
El usuario recibirá el mensaje con tu nombre de usuario como remitente.

El usuario receptor también puede responder usando `write`. Por ejemplo:
```bash
write iafzal
```

Mensaje de respuesta:
```bash
Está bien, Imran, haré lo mejor que pueda.
Gracias, spider.
```

Presiona **Ctrl + D** para finalizar el mensaje.

---

## Importancia de la Comunicación como Administrador del Sistema

Generalmente, esta comunicación se realiza desde la cuenta **root**, que tiene los mayores privilegios.

El usuario root puede reiniciar, apagar el sistema o realizar tareas de mantenimiento, y **debe** notificar adecuadamente a los usuarios conectados.

Puedes practicar iniciando múltiples sesiones y probando estos comandos.  
Son útiles tanto en un entorno de aprendizaje como en uno profesional.

---

## Puntos Clave

- El comando `users` muestra todos los usuarios actualmente conectados al sistema Linux.
    
- El comando `wall` permite **enviar un mensaje a todos los usuarios conectados** al mismo tiempo.
    
- El comando `write` permite enviar un mensaje privado a **un usuario específico**.
    
- La comunicación con los usuarios es esencial para tareas de **mantenimiento del sistema y gestión de recursos**.


---

## Introducción a los Permisos Especiales

En esta lección aprenderemos sobre los **permisos especiales** que se pueden asignar a archivos y directorios en Linux usando:

- **setuid**
    
- **setgid**
    
- **sticky bit**
    

Cuando se listan archivos y directorios, verás que cada uno tiene permisos de **lectura, escritura y ejecución** divididos en tres grupos:

1. El primer grupo de bits es para el **usuario propietario**.
    
2. El segundo, para el **grupo**.
    
3. El tercero, para **otros usuarios**.
    

Todos estos son conocidos como **bits de permiso**, y se pueden modificar usando el comando `chmod`.

---

## Visión General de los Permisos Adicionales

En Linux existen tres permisos especiales adicionales:

- **setuid**: Indica que el programa debe ejecutarse con el ID de usuario efectivo del **propietario**, no del usuario que lo ejecuta.
    
- **setgid**: Indica que el programa se ejecutará con el ID de grupo efectivo del **propietario**, no del ejecutor.
    
- **sticky bit**: Permite que solo el **propietario o root** puedan eliminar archivos o directorios protegidos con este bit.
    

---

## Permiso setuid

Un ejemplo común es el comando `passwd`: cuando un usuario lo ejecuta, en realidad corre como **root**, ya que se necesita modificar el archivo `/etc/shadow`, y un usuario normal no tiene permisos sobre ese archivo.

Por eso, el bit **setuid** está asignado al programa `passwd`.

---

## Permiso setgid

El bit **setgid** indica que el programa se ejecutará con el ID de grupo del **propietario del archivo**, no del usuario que lo corre.

Ejemplos de comandos con setgid son `locate` y `wall`.  
Puedes ver estos bits ejecutando `ls -l` sobre estos comandos.

> **Nota**: estos bits **solo aplican a archivos ejecutables**.  
> Además, **setuid y setgid no son comandos**; se configuran con `chmod`.

---

## Sticky Bit

El **sticky bit** se aplica a directorios o archivos para que **solo el propietario o root puedan eliminarlos**.

Este permiso es útil para proteger directorios compartidos como `/tmp`, evitando que usuarios eliminen archivos ajenos.

---

## Asignar y Eliminar Permisos Especiales

- Para asignar **setuid** a un programa:
```bash
chmod u+s <programa>
```

- Para asignar **setgid**:
    
```bash
chmod g+s <programa>
```

- Para eliminar estos permisos:
```bash
chmod u-s <programa> chmod g-s <programa>
```

---

## Buscar Archivos con setuid y setgid

Puedes encontrar todos los ejecutables con permisos setuid o setgid ejecutando:
```bash
find / -perm /6000 -type f
```

---
## Demostración de setuid con el Comando passwd

Si ejecutas:
```bash
which passwd
```

Verás que está en `/usr/bin`. Luego:
```bash
ls -l /usr/bin/passwd
```

Verás una **“s” mayúscula (S)** en la sección de permisos del usuario, lo que indica que el bit setuid está activo.

Cuando un usuario normal ejecuta `passwd`, el proceso se ejecuta con permisos de **root**, gracias a ese bit.

---

## Listar Comandos con Permisos Especiales

Como root, ejecuta:
```bash
find / -perm /6000 -type f
```

Esto mostrará todos los comandos con **setuid o setgid**.

Por ejemplo:
```bash
ls -l /usr/bin/locate
```

Te mostrará el bit **setgid** activado.

---

## Nota Importante: Aplicabilidad

Estos bits **solo funcionan en ejecutables compilados** (como en C o C++).  
**No funcionan en scripts de bash**.

---

## Entendiendo el Sticky Bit en Directorios

El sticky bit aparece en el último bit de los permisos.  
Por ejemplo, en el string de permisos, verás una **“t” minúscula o mayúscula** al final.

Este bit es útil para directorios como `/tmp`, donde **todos los usuarios pueden escribir**, pero **solo el propietario o root pueden borrar** archivos.

---

## Ejercicio Práctico: Usar el Sticky Bit

1. Entra como **root** y crea un directorio:
```bash
mkdir /allinone
```

2. Asigna todos los permisos:
```bash
chmod 777 /allinone
```

3. Cambia al usuario `iafzal` y crea un subdirectorio:
```bash
mkdir /allinone/Imrandir chmod 777 /allinone/Imrandir touch a b c
```

4. Desde otra terminal, entra como `spiderman` y elimina `Imrandir`:
```bash
rm -rf /allinone/Imrandir
```

**Se eliminará**, ya que aún no hay sticky bit.

5. Vuelve a root y asigna el sticky bit:
```bash
chmod +t /allinone ls -ld /allinone
```

Verás una `t` al final de los permisos.

6. Como `iafzal`, vuelve a crear el directorio `Imrandir` y los archivos.
    
7. Como `spiderman`, intenta eliminarlos:
    
```bash
rm -rf /allinone/Imrandir 
```

**Fallará** con el error: “**Operation not permitted**”, ya que el sticky bit protege el directorio.

---

## Resumen

Aunque el directorio `/allinone` tiene permisos de lectura, escritura y ejecución para todos, el **sticky bit** asegura que **solo el propietario o root** puedan eliminar su contenido.

---

## Puntos Clave

- Los permisos especiales en Linux incluyen `setuid`, `setgid` y `sticky bit`, los cuales alteran el comportamiento de archivos y directorios en relación con propiedad y permisos.
    
- `setuid`: permite que un programa se ejecute con el ID de usuario del propietario (ej. `passwd`).
    
- `setgid`: ejecuta el programa con el ID de grupo del propietario (ej. `wall`, `locate`).
    
- `sticky bit`: en directorios, impide que otros usuarios borren archivos que no les pertenecen.
    
- Se configuran y eliminan usando `chmod` con los flags `+s`, `-s`, `+t`, etc.
    
- Solo aplican a archivos binarios compilados (C/C++), **no** a scripts de shell.
    

---

## Autenticación de Cuentas en Linux

La **autenticación de cuentas en Linux** implica la creación de cuentas de usuario locales en una máquina Linux.  
Normalmente, estas cuentas se crean directamente en el sistema local.

Sin embargo, cuando se gestionan **miles de usuarios**, crear y mantener cuentas locales en **cada servidor individual** se vuelve ineficiente e impráctico.

### Ejemplo:

Si un nuevo empleado ingresa a la empresa, no sería viable crear su cuenta manualmente en **mil servidores**, iniciando sesión en cada uno.

---

## ¿Cómo se Resuelve Este Problema?

En entornos **Windows**, esto se soluciona usando **servicios de directorio** o **servidores de directorio**, que permiten autenticar usuarios desde un servidor **centralizado**, en lugar de hacerlo máquina por máquina.

---

## Tipos de Cuentas de Usuario

Existen **dos tipos básicos** de cuentas de usuario:

1. **Cuentas locales**:
    
    - Se crean directamente en la máquina.
        
    - Ejemplo: usando el comando `useradd` en Linux.
        
2. **Cuentas de directorio**:
    
    - Se gestionan **centralmente** en un **servidor de directorio**.
        
    - Se utilizan para autenticarse en múltiples máquinas.
        

La mayoría de las cuentas en Linux son **locales**, porque Linux se utiliza principalmente para administración del sistema o para ejecutar servicios y aplicaciones.

---

## Autenticación con Servidor de Directorio

Cuando se utilizan **cuentas de directorio**, todas las cuentas de usuario se almacenan en una **base de datos centralizada** en un **servidor de directorio**.

Cuando un usuario intenta iniciar sesión en una máquina cliente:

- La solicitud de autenticación se envía al **servidor de directorio**.
    
- Si la cuenta existe y las credenciales son válidas, el servidor **autentica al usuario** y permite el acceso.
    

Este proceso **solo aplica a cuentas de directorio**, **no** a cuentas locales creadas directamente en las máquinas.

---

## Active Directory en Windows

Windows utiliza un servicio de directorio llamado **Microsoft Active Directory**.  
Este sistema gestiona la autenticación de usuarios de forma eficiente en entornos corporativos.

- Los usuarios se crean en el **servidor de Active Directory**.
    
- Las máquinas clientes envían sus solicitudes de autenticación a este servidor.
    

---

## Protocolos de Autenticación

- Para iniciar sesión en servidores, se utiliza comúnmente el protocolo **SSH**.
    
- Para autenticación mediante servicios de directorio, se utiliza el protocolo **LDAP**.
    

**LDAP** significa _Lightweight Directory Access Protocol_ (Protocolo Ligero de Acceso a Directorios) y es un **protocolo**, no un servicio de directorio en sí mismo.

---

## Aclaración sobre LDAP

Es un error común pensar que **Linux usa LDAP como servicio de directorio**.  
En realidad:

- **LDAP no es un servicio**, es **solo un protocolo**.
    
- Es utilizado por sistemas como **Windows**, **Linux** y **Mac** para autenticarse frente a servicios de directorio (como Active Directory, OpenLDAP, etc.).
    
- **Linux no tiene un servicio de directorio llamado LDAP**. Solo usa el protocolo para comunicarse con uno.
    

---
## Puntos Clave

- Las **cuentas locales en Linux** se crean por separado en cada máquina, lo cual no es práctico para grandes volúmenes de usuarios.
    
- Los **servicios de directorio** permiten centralizar la autenticación de usuarios a través de un solo servidor.
    
- **LDAP** es un **protocolo de autenticación**, no un servicio de directorio.
    
- **Active Directory** es un servicio de directorio nativo de Windows, ampliamente utilizado en entornos empresariales.

---

## Diferencias entre Active Directory, LDAP, IDM, WinBIND, OpenLDAP, etc.

### Introducción a los Servicios de Directorio

Esta lección aborda las diferencias entre **Active Directory**, **LDAP**, **IDM**, **WinBIND** y **OpenLDAP**.  
Muchas personas se confunden sobre qué son estos servicios y protocolos de directorio, y cuál deberían usar en sus sistemas.

Por ejemplo:  
Si tienes Linux, ¿deberías usar **Active Directory** o **LDAP**?

Esta lección tiene como objetivo aclarar estas dudas y ayudarte a implementar el servicio de directorio adecuado para tu entorno.

---

### Active Directory

**Active Directory** es un producto desarrollado y propiedad de **Microsoft**.  
Está diseñado para entornos con **miles de computadoras Windows**, donde las cuentas de usuario deben autenticarse contra un servidor de Active Directory.

Es especialmente importante en entornos **Windows**, más que en Linux.

En Linux, normalmente se usa en entornos empresariales por administradores de sistemas, desarrolladores o personal de QA — es decir, por un **número reducido de usuarios**.  
Los usuarios suelen iniciar sesión a través de aplicaciones, no directamente en el sistema.

📌 **Ejemplo**:  
Cuando inicias sesión en Facebook (que corre sobre servidores Linux), tu información no se guarda localmente en el servidor.  
En cambio, se guarda en una **base de datos**, y tu autenticación es gestionada por otra máquina, no por una cuenta local en Linux.  
De forma similar, Windows usa **Active Directory** como su sistema de autenticación.

---

### IDM (Identity Manager)

Active Directory es para entornos Windows, pero ¿qué pasa en entornos Linux?

**Red Hat**, una distribución empresarial de Linux, desarrolló **IDM (Identity Manager)** para gestionar usuarios en entornos Linux.

Aunque Linux normalmente tiene pocos usuarios, en empresas grandes con muchos administradores de sistemas se requiere una **gestión centralizada** de usuarios, igual que en Windows.

IDM cumple ese propósito en Linux.

- IDM está disponible en **Red Hat**.
    
- Algunas funciones pueden estar disponibles en **CentOS**, pero no está claro si el producto completo lo está.
    
- Puedes buscar más información sobre IDM en línea.
    

---

### WinBIND

**WinBIND no es un servicio de directorio en sí**, sino un **componente de Samba**, un software de código abierto para Linux.

WinBIND permite que **usuarios de Active Directory de Windows** se autentiquen en máquinas Linux.

➡️ ¿Por qué es necesario?  
Porque Windows no puede comunicarse directamente con Linux.  
**WinBIND actúa como intermediario** entre una máquina Linux y Active Directory.

- Al instalar y configurar WinBIND en Linux, permites que usuarios de Active Directory puedan iniciar sesión en Linux.
    
- La configuración es sencilla y hay abundante documentación en línea.
    

---

### OpenLDAP y el Protocolo LDAP

Es importante **distinguir entre LDAP y OpenLDAP**:

- **LDAP** significa _Lightweight Directory Access Protocol_ (Protocolo Ligero de Acceso a Directorios).  
    Es un **protocolo**, no un servicio.  
    Se usa para comunicarse con servicios de directorio.
    
- **OpenLDAP** es una **implementación de código abierto** de un servicio de directorio que utiliza el protocolo LDAP.
    

➡️ En Linux, **OpenLDAP** funciona como un servicio de directorio similar a Active Directory o IDM, pero de **código abierto**.

- IDM (de Red Hat) es un producto de pago.
    
- OpenLDAP puede descargarse e instalarse libremente, por ejemplo:
    
```bash
yum install openldap
```

La instalación y configuración de OpenLDAP se cubrirá más adelante en el módulo 7 (sobre acceso a internet en Linux y gestión de paquetes).

---

### Otros Servicios de Directorio

Existen otros productos como:

- **IBM Directory Services**, un servicio propietario vendido por IBM.
    
- **JumpCloud**, que ofrece servicios de directorio **basados en la nube**.
    

🟡 Recordatorio:  
**LDAP no es un servicio de directorio**, sino **un protocolo** que utilizan los servicios de directorio (ya sea en Linux o Windows) para autenticar usuarios.

---

## Puntos Clave

- **Active Directory** es un producto de Microsoft, usado principalmente en entornos grandes con usuarios Windows.
    
- **IDM** es la alternativa de Red Hat para la gestión de usuarios en entornos Linux empresariales.
    
- **WinBIND** permite que máquinas Linux autentiquen usuarios de Active Directory.
    
- **LDAP** es un protocolo (no un servicio de directorio).
    
- **OpenLDAP** es un servicio de directorio libre que utiliza el protocolo LDAP.

---

## Comandos de Utilidad del Sistema en Linux

### Introducción

Los **comandos de utilidad del sistema** son comandos básicos que se utilizan frecuentemente en entornos Linux. Algunos de ellos tienen funciones similares a las herramientas disponibles en Windows, como la fecha y el calendario.

Sin embargo, mientras que en **Windows** estas funciones están disponibles mediante una interfaz gráfica (GUI), en **Linux** se accede principalmente por consola. Por ello, se han desarrollado comandos específicos para mostrar información como la fecha, el tiempo de actividad del sistema, el nombre del host y detalles del sistema operativo.

---

## Acceder a Información del Sistema en Linux

Cuando inicias sesión en una máquina Linux con consola gráfica, puedes ver la hora y el calendario automáticamente.

Pero si accedes a través de un cliente de terminal como **PuTTY**, no tendrás acceso a la interfaz gráfica.  
La mayoría de las veces, trabajarás en entornos **sin GUI**, por lo que es fundamental conocer estos comandos.

---

## Conectarse a la Máquina Linux

Para conectarte, introduce la dirección IP de la máquina (por ejemplo: `192.168.56.101`).

Si no recuerdas la IP:

- Usa `ifconfig` (en sistemas antiguos).
    
- Usa `ip a` (en versiones nuevas de Red Hat o CentOS).
    

Una vez que accedas, inicia sesión con tus credenciales. Verás una terminal en blanco lista para usar comandos.

---

## Comando `date`

Para mostrar la **fecha y hora actual**, escribe:
```bash
date
```

Este comando mostrará:

- Día de la semana
    
- Fecha
    
- Hora (incluyendo segundos)
    
- Zona horaria
    
- Año
    

También se usa en scripts para registrar cuándo se ejecutan tareas.

---

## Comando `uptime`

Muestra cuánto tiempo lleva el sistema funcionando, cuántos usuarios están conectados y el **promedio de carga** del sistema:
```bash
uptime
```

---

## Comando `hostname`

Muestra el **nombre del host** (nombre del equipo):
```bash
hostname
```

Recomendado para confirmar en qué sistema estás antes de ejecutar comandos críticos.

---

## Comando `uname`

Proporciona información sobre el **sistema operativo**:
```bash 
uname
```

Para obtener información más detallada:
```bash
uname -a
```

sto incluye:

- Nombre del host
    
- Versión del kernel
    
- Fecha de compilación
    
- Arquitectura del sistema
    

---

## Comando `which`

Este comando muestra la **ruta del archivo ejecutable** de un comando. Por ejemplo:
```bash
which pwd
```

Resultado típico:
```bash
/usr/bin/pwd
```

Para ver los **atributos del archivo**:
```bash
ls -l /usr/bin/pwd
```

Muestra información sobre:

- Permisos
    
- Propietario
    
- Grupo
    
- Si tiene permisos de ejecución
    

---

## Localizar Otros Comandos

Para encontrar la ubicación del comando `date`:
```bash
which date
```

La mayoría de los comandos están en `/usr/bin`.
Para listar todos:
```bash
ls -l /usr/bin
```

Para ver una página a la vez:
```bash
ls -l /usr/bin | more
```

---

## Contar Cuántos Comandos Hay

Para saber cuántos comandos hay en `/usr/bin`:
```bash
ls -l /usr/bin | wc -l
```

Ejemplo de resultado: `1597` comandos disponibles en esa carpeta.

---

## Comando `cal`

Muestra el **calendario del mes y año actual**:
```bash
cal
```

Para un mes y año específicos:
```bash
cal 9 1977
```

Muestra que el **18 de septiembre de 1977 fue domingo**.

Para mostrar todos los meses de un año:
```bash
cal 2016
```

---

## Comando `bc` (calculadora)

`bc` significa _binary calculator_. Es una calculadora de línea de comandos.

Para usarla:
```bash
bc
```

Una vez dentro, puedes hacer operaciones como:
```bash
2+2
256*321
```

Para salir:
```bash
quit
```

---
## Puntos Clave

- Los comandos de utilidad del sistema en Linux brindan información esencial como **fecha, tiempo activo, nombre del sistema y detalles del sistema operativo**.
    
- El comando `which` localiza ejecutables, y `ls -l` muestra atributos del archivo.
    
- `cal` muestra calendarios por mes o año, y `bc` es una calculadora dentro del terminal.
    
- Son esenciales cuando trabajas en **entornos sin interfaz gráfica**, como PuTTY o conexiones SSH.


---

## Redimensionamiento de disco

Usamos vgs para revisar cuando espacio tenemos disponible:

```bash
vgs
  VG        #PV #LV #SN Attr   VSize    VFree   
  ubuntu-vg   1   1   0 wz--n- <473.89g <373.89g

```

Con lvextend podemos extender el disco, en mi caso necesito pasarle la option -l para pedirle que tome el 100% del espacio libre para el redimencionamiento

```bash
lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
```

Finalmente podemos confirmar el cambio en caliente con resize2fs (para particiones tipo ext4)

```bash
resize2fs /dev/ubuntu-vg/ubuntu-lv
```

Validación:
```bash
df -Th /
Filesystem                        Type  Size  Used Avail Use% Mounted on
/dev/mapper/ubuntu--vg-ubuntu--lv ext4  466G   93G  354G  21% /

df -h
Filesystem                         Size  Used Avail Use% Mounted on
tmpfs                              3.2G  1.8M  3.2G   1% /run
efivarfs                           192K   93K   95K  50% /sys/firmware/efi/efivars
/dev/mapper/ubuntu--vg-ubuntu--lv  466G   93G  354G  21% /
tmpfs                               16G     0   16G   0% /dev/shm
tmpfs                              5.0M     0  5.0M   0% /run/lock
/dev/nvme0n1p2                     2.0G  190M  1.6G  11% /boot
/dev/nvme0n1p1                     1.1G  6.2M  1.1G   1% /boot/efi
overlay                            466G   93G  354G  21% /var/lib/docker/overlay2/70aa3e67da66d2ee914905cdbaf8c275d977d2a9b6114a17255045cff015a255/merged
tmpfs                              3.2G   20K  3.2G   1% /run/user/1000

```


