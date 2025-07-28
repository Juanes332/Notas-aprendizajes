---
id: 2025072801
tags:
  - Cibersecurity
  - Linux
  - Console
  - English
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

1. Hacer clic en la pestaña **Proxy** para ver los submenús.    
2. Ir a la subpestaña **Intercept**.    
3. **Desactivar temporalmente el modo Intercept** para navegar sin interrupciones.


### Consejos sobre el uso de Intercept:

- Con **Intercept activado**, cada solicitud debe ser aprobada manualmente con el botón **Forward** o descartada con **Drop**.
- Esto es útil para modificar tráfico, pero puede resultar **tedioso al navegar normalmente**.
- Por ello, se recomienda desactivarlo durante el reconocimiento pasivo o navegación común y **activarlo solo cuando se necesite interceptar y modificar solicitudes específicas**.

![[Pasted image 20250728163522.png]]

A continuación, podemos revisar la configuración del receptor de proxy. La subpestaña "Options" muestra qué puertos están escuchando las solicitudes de proxy.

![[Pasted image 20250728163558.png]]

### Configuración del navegador Firefox para usar Burp Suite como proxy

Por defecto, **Burp Suite activa un proxy listener** en la dirección `localhost:8080`. Para redirigir el tráfico web a través de Burp, el navegador debe estar configurado para conectarse a ese **host y puerto**.

### Nota sobre el navegador integrado

- Burp ahora incluye su **propio navegador basado en Chromium**, preconfigurado para usar todas sus funciones.
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