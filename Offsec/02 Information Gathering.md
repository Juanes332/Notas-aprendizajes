
Intro:

The goal of a penetration test (or pentest) is to detect security gaps to improve the defenses of the client organization. Because the network, devices, and software within the company's environment change over time, penetration testing is a cyclic activity. A company's attack surface changes periodically due to newly-discovered software vulnerabilities, configuration mistakes from internal activities, or IT restructuring that might expose new segments for targeting.

Additionally, changes such as onboarding new employees, introducing new products, deploying new machines, expanding infrastructure, or launching websites can further broaden the attack surface, necessitating regular reassessments.


## The Penetration Testing Lifecycle

**Learning Objectives:**

- Understand the stages of a penetration test
    
- Learn the role of information gathering in each stage
    
- Differentiate between active and passive information gathering
    

**Penetration Test Stages:**

1. **Defining the Scope** – Identify what is in and out of scope (e.g., IP ranges, systems).
    
2. **Information Gathering** – Collect data about the target using passive and active methods.
    
3. **Vulnerability Detection** – Identify weaknesses.
    
4. **Initial Foothold** – Gain access to the target.
    
5. **Privilege Escalation** – Increase access level.
    
6. **Lateral Movement** – Move through the network to other systems.
    
7. **Reporting/Analysis** – Document findings and results.
    
8. **Lessons Learned/Remediation** – Review and suggest fixes.
    

**Key Points on Scoping:**

- Scope defines what can and cannot be tested.
    
- Must be agreed upon before testing starts.
    

**Information Gathering:**

- Begins after scoping.
    
- Objective: collect detailed info on the target (infrastructure, assets, personnel).
    
- **Passive Reconnaissance:** No direct interaction (e.g., WHOIS, social media, DNS queries).
    
- **Active Reconnaissance:** Direct probing of the target (e.g., port scanning).
    
- Passive = lower risk of detection; Active = higher visibility.
    
- Enumeration continues throughout the test (not just at the beginning).
    

**Takeaway:**

- Information gathering is continuous and critical to discovering the full attack surface.
    
- The balance between passive and active methods depends on stealth requirements and objectives.

---

## Passive Information Gathering

**Learning Objectives:**

- Understand the two approaches to passive information gathering
    
- Learn about Open-Source Intelligence (OSINT)
    
- Understand web server and DNS passive information gathering
    


### **What is Passive Information Gathering?**

- Also known as **OSINT (Open-Source Intelligence)**
    
- Involves collecting publicly available information **without alerting the target**
    
- Goal: maintain a **low footprint** while identifying useful data to assist later attack stages (e.g., phishing, password guessing)
    


### **Two Interpretations of "Passive":**

1. **Strict Passive Gathering:**
    
    - **No direct contact** with the target's systems
        
    - Uses **third-party sources** only (e.g., search engines, public records)
        
    - Maximizes stealth but may limit information
        
2. **Loose Passive Gathering:**
    
    - May interact with the target like a **regular user**
        
    - Examples: viewing websites, creating an account, reading public posts
        
    - Still avoids **scanning, probing**, or testing for vulnerabilities
        

✅ _This module adopts the looser interpretation._


### **Nature of Passive Recon:**

- **Cyclical**, not linear — findings lead to new techniques or searches
    
- **Flexible and creative** process
    
- Each tool/resource can reveal new data points that open other pathways
    


### **Main Goals of Passive Recon:**

- **Expand the attack surface**
    
- Support **social engineering** (e.g., phishing)
    
- Enable **credential guessing/brute force** attacks
    
- Identify:
    
    - Email addresses
        
    - Phone numbers
        
    - Internal systems
        
    - Employee behavior
        
    - Company policies or technology stack
        


### **Real-World Example: OffSec Passive Recon Success Story**

**Target:** Small company with minimal online footprint

- Few exposed services, all secure — traditional probing yielded no results
    
- Using **Google dorking** and public forums, discovered a post by an employee ("David") using their **corporate email** and **personal phone** on a **stamp collecting forum**
    

**Steps Taken:**

1. Registered a **fake stamp-trading domain**
    
2. Created a **believable stamp site** (Google Images)
    
3. Embedded **client-side exploit code**
    
4. Called "David" pretending to be a fellow collector
    
5. "David" visited the site from his work machine
    
6. Exploit delivered a **reverse shell** to the team
    

**Key Takeaways:**

- Minor public info (like forum posts) can lead to **serious compromise**
    
- **Security awareness** and **company policies** are as important as technical defenses
    
- Penetration testers **avoid blaming individuals**; the goal is to improve **system-wide security**
    


### **Ethical Considerations:**

- Always follow the **rules of engagement**
    
- Avoid targeting specific employees in reports
    
- Focus on improving the **organization’s security posture**, not blaming people
    


### **Next Steps:**

- Study popular OSINT tools and techniques
    
- Apply them in a simulated campaign against **MegaCorp One** (fictional company)


---


## 1. Whois Enumeration

[_Whois_](https://www.domaintools.com/support/what-is-whois-information-and-why-is-it-valuable/) es un servicio, herramienta y tipo de base de datos TCP que puede proporcionar información sobre un nombre de dominio, como el [_name server_](https://www.forbes.com/advisor/business/software/what-is-a-name-server/) y [_registrar_](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name-registrar/). Esta información suele ser pública, ya que los registradores cobran una tarifa por el registro privado.

Podemos recopilar información básica sobre un nombre de dominio ejecutando una búsqueda estándar hacia adelante y pasando el nombre de dominio, **megacorpone.com**, usando`whois`, Proporcionar la dirección IP de nuestro servidor WHOIS de Ubuntu como argumento del host  y el parametro (`-h`)

``` bash
kali@kali:~$ whois megacorpone.com -h 192.168.50.251
   Domain Name: MEGACORPONE.COM
   Registry Domain ID: 1775445745_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.gandi.net
   Registrar URL: http://www.gandi.net
   Updated Date: 2019-01-01T09:45:03Z
   Creation Date: 2013-01-22T23:01:00Z
   Registry Expiry Date: 2023-01-22T23:01:00Z
...
Registry Registrant ID: 
Registrant Name: Alan Grofield
Registrant Organization: MegaCorpOne
Registrant Street: 2 Old Mill St
Registrant City: Rachel
Registrant State/Province: Nevada
Registrant Postal Code: 89001
Registrant Country: US
Registrant Phone: +1.9038836342
...
Registry Admin ID: 
Admin Name: Alan Grofield
Admin Organization: MegaCorpOne
Admin Street: 2 Old Mill St
Admin City: Rachel
Admin State/Province: Nevada
Admin Postal Code: 89001
Admin Country: US
Admin Phone: +1.9038836342
...
Registry Tech ID: 
Tech Name: Alan Grofield
Tech Organization: MegaCorpOne
Tech Street: 2 Old Mill St
Tech City: Rachel
Tech State/Province: Nevada
Tech Postal Code: 89001
Tech Country: US
Tech Phone: +1.9038836342
...
Name Server: NS1.MEGACORPONE.COM
Name Server: NS2.MEGACORPONE.COM
Name Server: NS3.MEGACORPONE.COM
...
```


data:

``` bash
kali@kali:~$ whois 38.100.193.70 -h 192.168.50.251
...
NetRange:       38.0.0.0 - 38.255.255.255
CIDR:           38.0.0.0/8
NetName:        COGENT-A
...
OrgName:        PSINet, Inc.
OrgId:          PSI
Address:        2450 N Street NW
City:           Washington
StateProv:      DC
PostalCode:     20037
Country:        US
RegDate:        
Updated:        2015-06-04
...
```


---

## 2. Google hacking

El termino de "google hacking" fue popularizado por Johnny Long en 2001. Por medio de conferencias y su popular libro Google [Hacking For Penetration Testers](https://www.amazon.com/Google-Hacking-Penetration-Testers-Johnny/dp/0128029641/ref=dp_ob_image_bk) explica como podemos usar las búsquedas por google para encontrar vulnerabilidades criticas, información sensible y sitios web con falta de configuraciones.

En el corazón de esta tecnica se encuentran las formas inteligentes de armar cadenas de busqueda y los **operadores** para el refinamiento creativo de estas busquedas.
Este proceso inicia con una busqueda iterativa de la información que queremos obtener, donde empezamos a usar los diferentes operadores para eliminar lo irrelevante de nuestra busqueda.

Para empezar veremos el operador *site*. Este limita las busquedas a algo que le pasemos en concreto. Por ejemplo si solo quiero buscar el dominio cloudflare.com en el panel de busqueda escribimos site:cloudflare.com.

![[Pasted image 20250702121212.png]]

En esta imagen podemos observar como el operador site limita las busquedas a megacorpone.com

Podemos utilizar otros operadores para limitar nuestros resultados. Por ejemplo, el operador filetype (o ext) limita los resultados de busqueda a un tipo de archivo en concreto.

En el siguiente ejemplo veremos como vamos a combinar operadores para encontrar archivos tipo txt (filetype:txt) en www.megacorpone.com (site:megacorpone.com)

![[Pasted image 20250702121702.png]]

Hemos encontrado un resultado interesante. Nuestra consulta nos retorna el archivo robots.txt, que contiene el siguiente contenido:

```txt
User-agent: *
Allow: /
Allow: /nanites.php
```

Este archivo le dice al web crawler (que es un bot que recopila información de paginas web para los motores de busqueda) que permita o no permita recursos específicos. En este caso podemos ver que tenemos una pagina específica de PHP (/nanites.php) que de otro modo estaría oculta en la busqueda normal a pesar de estar permitido por la política de busqueda.

El operador ext nos puede servir para saber que lenguaje de programación está usando el website. Busquedas como **ext:php**, **ext:xml**, and **ext:py** nos pueden ayudar a encontrar paginas indexadas de PHP, XML y python respectivamente.

También podemos modificar los operadores usando "-" para excluir items de una busqueda reduciendo los resultados.

Por ejemplo, para encontrar paginas interesantes que no sean html podemos usar **site:megacorpone.com** para limitar los resultados de busqueda a **megacorpone.com** y sus sub dominios, seguido por **-filetype:html** para excluir las paginas HTML de los resultados

![[Pasted image 20250702123828.png]]


En otro ejemplo, podemos utilizar una búsqueda de **intitle:"index of" "parent directory"** para encontrar páginas que contengan "índice de" en el título y las palabras "directorio principal" en la página.

![[Pasted image 20250702124327.png]]

La salida se refiere a páginas de [_directory listing_](https://cwe.mitre.org/data/definitions/548.html) que muestran el contenido de los archivos sin páginas de índice. Configuraciones incorrectas como esta pueden revelar archivos interesantes e información confidencial.

Estos ejemplos básicos son sólo una pequeña muestra de lo que podemos hacer con los operadores de búsqueda.

La  [_Google Hacking Database_](https://www.exploit-db.com/google-hacking-database) (GHDB) contiene multitud de búsquedas creativas que demuestran el poder de aprovechar operadores combinados.

![[Pasted image 20250702124549.png]]

Otras formas que tenemos de experimentar con los google dorks es a través del portal [_DorkSearch_](https://dorksearch.com/).

---

## 2.3 Netcraft

[_Netcraft_](https://www.netcraft.com/) es una empresa de servicios de Internet con sede en Inglaterra que ofrece un portal web gratuito que realiza varias funciones de recopilación de información, como descubrir qué tecnologías se están ejecutando en un sitio web determinado y encontrar qué otros hosts comparten el mismo bloque de red IP.

El uso de servicios como Netcraft se considera una técnica pasiva, ya que nunca interactuamos directamente con nuestro objetivo.

En el siguiente ejemplo podemos visualizar como obtenemos información sobre el dominio megacorpone.com usando esta herramienta [_DNS search page_](https://searchdns.netcraft.com) de netcraft

![[Pasted image 20250702125104.png]]

Para cada servidor encontrado, podemos ver un "informe del sitio" que proporciona información adicional e historial sobre el servidor haciendo clic en el ícono de archivo junto a cada URL del sitio.

![[Pasted image 20250702125220.png]]

El informe comienza con la información de registro. Sin embargo, si nos desplazamos hacia abajo, descubrimos varias entradas de "site technology".

![[Pasted image 20250702125323.png]]

Durante el 2024 netcraft a decidido quitar estas funcionalidades, si queremos ver tamibién información detallada podemos usar la herramienta de wappalyzer:
https://www.wappalyzer.com/lookup/megacorpone.com/

---

## 2.4 Open-Source Code

Estas son algunas herramientas donde podemos encontrar proyectos open-source y además podemos usar para obtener información de manera pasiva:

- [_GitHub_](https://github.com/)
- [_GitHub Gist_](https://gist.github.com/)
- [_GitLab_](https://about.gitlab.com/)
- [_SourceForge_](https://sourceforge.net/)

La busqueda de gihub es bastante sensible, podemos usarla para la busqueda de archivos, contraseñas, api keys, keys de aws, etc. Esta es la forma en la que podemos hacerlo:

![[Pasted image 20250702130417.png]]


Podemos buscar por dueño (owner) en este caso megacorpone y usamos el path de users. 
Acá podemos ver que nos devolvió un archivo.

![[Pasted image 20250702130424.png]]


Esto es interesante porque xampp es un entorno de desarrollo para apps web, este es su contenido.

![[Pasted image 20250702130659.png]]

Este archivo contiene el user_name y la contraseña en hash. Esto nos va a ayudar bastante cuando estemos iniciando nuestra fase de ataque activo.

Este enfoque manual funcionará mejor con repositorios pequeños. Para repositorios más grandes, podemos usar varias herramientas para automatizar algunas búsquedas, como Gitrob y Gitleaks. La mayoría de estas herramientas requieren un token de acceso para usar la API del proveedor de alojamiento de código fuente.

La siguiente imagen muestra un ejemplo Gitleaks encontrando un [_AWS access key ID_](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) en un archivo

![[Pasted image 20250702131511.png]]

Obtener estas credenciales nos da acceso ilimitado a la cuenta de AWS, lo cual puede comprometer todos los servicios de la compañía que estén desplegados en esa cuenta.

---

## 2.5 Shodan

Shodan es un motor de busqueda que usa un device crawler para analizar y obtener información de dispositivos conectados a internet. Este navegador nos puede servir bastante para obtener información relevante de manera pasiva.

Empecemos buscando **hostname:megacorpone.com**

![[Pasted image 20250702131931.png]]

En este caso shodan nos da una lista de ips, puertos e información pasiva que obtiene interactuando directamente con el sitio web.

Tambien podemos especificar el puerto en especifico, en este caso vamos a buscar por el puerto 22.

![[Pasted image 20250702132138.png]]

En base a los resultados de shodan ahora sabemos exactamente la versión de openSSH que está corriendo en cada servidor. Esta información es útil para buscar vulnerabilidades de esa versión y lograr tener acceso al server por ejemplo.

![[Pasted image 20250702132308.png]]

 En esta página, podemos revisar los puertos, servicios y tecnologías que utiliza el servidor. Shodan también revelará si existen vulnerabilidades publicadas para alguno de los servicios o tecnologías identificados que se ejecutan en el mismo host. Esta información es fundamental para determinar por dónde empezar al iniciar las pruebas activas.

---

## 2.6. Security Headers and SSL/TLS

[_Security Headers_](https://securityheaders.com/) analizará los encabezados de respuesta HTTP y proporcionará un análisis básico de la seguridad del sitio objetivo. Podemos usar esto para comprender las prácticas de codificación y seguridad de una organización según los resultados.

![[Pasted image 20250702133457.png]]

Otra herramienta que podemos usar es [_Qualys SSL Labs_](https://www.ssllabs.com/ssltest/) . Esta herramienta nos ayuda a analizar la configuración SSL/TLS de nuestro server y la compara con las mejores practicas para identificar vulnerabilidades como [_Poodle_](https://www.cisa.gov/news-events/alerts/2014/10/17/ssl-30-protocol-vulnerability-and-poodle-attack) or [_Heartbleed_](https://heartbleed.com/).

![[Pasted image 20250702134131.png]]

![[Pasted image 20250702134146.png]]

Tenemos que revisar también que el TLS (Transport Layer Security) esté en versión 2 o 3 ya que este protocolo criptográfico se usa para proteger la información en internet y garantiza la confidencialidad y la integridad del mismo. Si el sitio tiene versión 1 debemos cambiarlo ya que actualmente es obsoleta por lo fácil que es descifrar los datos.

---

## 3. LLM-Powered Passive Information Gathering

---

#### 1. ¿Por qué usar LLMs en el reconocimiento pasivo?

- **Ventaja clave**: Un LLM puede digerir volúmenes enormes de texto (artículos, foros, redes sociales, documentos PDF, etc.) y destilar patrones o pistas que a un analista humano le tomaría horas o días localizar.
    
- **Casos típicos**:
    
    - Identificar contactos, políticas internas y tecnología utilizada por la organización.
        
    - Cruzar nombres de empleados y referencias en redes sociales para descubrir correo, cargo o proyectos.
        
    - Generar rápidamente conjuntos de Google dorks o ideas de phishing con un contexto muy preciso sobre la víctima.
        

---

#### 2. Riesgos y limitaciones a tener presentes

| Riesgo                                     | Descripción                                                    | Mitigación recomendada                                     |
| ------------------------------------------ | -------------------------------------------------------------- | ---------------------------------------------------------- |
| **Información desactualizada o inventada** | El modelo predice texto; no “verifica” datos.                  | Corroborar con fuentes primarias.                          |
| **Falta de contexto técnico**              | Puede malinterpretar siglas o jergas muy específicas.          | Formular prompts claros y revisar resultados.              |
| **Privacidad y confidencialidad**          | Los prompts viajan a la nube del proveedor.                    | Evitar incluir datos sensibles sin aprobación del cliente. |
| **Bias**                                   | El modelo refleja sesgos del corpus de entrenamiento.          | Revisar con mirada crítica; contrastar.                    |
| **Cumplimiento legal**                     | El uso del servicio puede quedar fuera de alcance contractual. | Confirmar con el cliente antes de usarlo.                  |

#### 3. Limitaciones de la versión gratuita de ChatGPT (enero 2025)

- Acceso básico a **GPT-3.5** y cupos reducidos a GPT-4, con respuestas más lentas y sin prioridad.
    
- Límites de uso menores y funciones avanzadas (por ejemplo, análisis de datos o DALL·E) restringidas a usuarios **Plus**.
    

---

#### 4. Ejemplos prácticos (prompts y respuestas)

> **4.1 WHOIS del dominio**  
> **Prompt:** `whois megacorpone.com`  
> **Respuesta resumida del LLM:**
> 
> - Dominio registrado: **22-ene-2013** (vence 22-ene-2025); registrador: **Gandi SAS**.
>     
> - Registrante: **Alan Grofield**, MegaCorpOne, 2 Old Mill St, Rachel (NV).
>     
> - Teléfono +1-903-883-6342 ‧ Email contacto@megacorpone.com.
>     
> - DNS: `ns1`, `ns2`, `ns3.megacorpone.com`.
>     
> - Estado: _clientTransferProhibited_ (bloqueo contra transferencias no autorizadas).
>     

> **Utilidad:** Nombre real + ubicación → insumo para OSINT y para pivotar a redes sociales; servidores DNS → vector de enumeración activa.

---

> **4.2 Estructura corporativa y empleados**  
> **Prompt:**  
> `Can you print out all the public information about company structure and employees of megacorpone?`  
> **Respuesta del LLM (extracto):**
> 
> - **CEO:** Joe Sheer — joe@megacorpone.com — Twitter `@Joe_Sheer`
>     
> - **VP Legal:** Mike Carlow — Tel. +5-***-03
>     
> - **Marketing Director:** Matt Smith — <msmith@…> — `@MattSmithMCO`
>     
> - … (hasta ~20 entradas)
>     
> - **Empleados totales:** ≈ 237  
>     **Utilidad:** Correos directos, cargos y handles de Twitter → materia prima para spear-phishing, vishing o ingeniería social.
>     

---

> **4.3 Lista de 20 Google dorks**  
> **Prompt:**  
> `can you provide the best 20 google dorks for megacorpone.com website tailored for a penetration test?`  
> **Respuesta (fragmento temático):**

> **Descubrimiento básico**
> 
> 1. `site:megacorpone.com` (todo el dominio)
>     
> 2. `site:megacorpone.com -www` (subdominios)
>     

> **Directorios y archivos**  
> 3. `site:megacorpone.com intitle:"index of"`  
> 4. `site:megacorpone.com ext:php | ext:html | ext:asp | ext:jsp`  
> 5. `site:megacorpone.com filetype:pdf | filetype:docx | filetype:xlsx`

> **Páginas sensibles**  
> 6. `site:megacorpone.com inurl:"admin"`  
> 7. `site:megacorpone.com inurl:"login"`  
> 8. `site:megacorpone.com inurl:"upload"`  
> … hasta el dork 20.

> **Bonus:** combinar con `cache:` o `related:` para versiones almacenadas o sitios similares.

---

> **4.4 Identificación del stack tecnológico**  
> **Prompt:** `Retrieve the technology stack of the megacorpone.com website`  
> **Respuesta del LLM:**
> 
> - **Frameworks & libs**: jQuery, Bootstrap, Font Awesome.
>     
> - **Lenguajes**: ECMAScript, JavaScript, HTML.
>     
> - **CDN**: Google Hosted Libraries.
>     
> - **Servidor web**: Apache HTTPD.
>     
> - **Hosting**: GitHub Pages.  
>     **Utilidad:** Enfocar pruebas en vulnerabilidades conocidas de Apache, dependencias JS o configuraciones CDN.
>     

---

#### 5. Conclusión

Los LLM son un multiplicador de productividad en **reconocimiento pasivo (OSINT)** porque:

1. **Agregan y correlacionan** datos dispersos más rápido que las herramientas tradicionales.
    
2. **Presentan resúmenes coherentes** que facilitan la toma de decisiones (por ejemplo, priorizar qué subdominios escanear o a qué empleado enviar un correo de phishing).
    
3. **Automatizan tareas repetitivas** ―generar dorks, enumerar tecnologías, redactar correos de spear-phishing― liberando tiempo del tester para actividades de mayor valor.
    

Sin embargo, su salida **no es infalible**: siempre se valida, se complementa con herramientas OSINT especializadas y se observa la normativa de seguridad y privacidad acordada con el cliente.

---


## 4. Active information Gathering

En esta sección aprendemos sobre obtener información de manera activa. Esta forma de obtener información es conocida por interactuar directamente con nuestro objetivo, por ejemplo un directorio activo, port scanning, enumeración de DNS, SMB, SMTP y SNMP.

En la mayoría de los casos realizaremos las pruebas de active information gathering en un sistema operativo como Kali o Parrot pero, durante los penetration testing no tendremos el lujo de hacerlo con nuestra herramienta de linux, asumiendo un escenario así normalmente nos dan una maquina windows-based workstation.

Cuando estamos fuera de nuestro entorno de linux, podremos aprovechar varios binarios de windows pre instalados y confiables para realizar el análisis posterior al ataque.
Estos binarios se abrevian como LOLBins o, más recientemente, LOLBAS para incluir binarios, scripts y bibliotecas.


---


## 4.1. DNS Enumeration

El [_Domain Name System_](https://www.cloudflare.com/learning/dns/what-is-dns/) (DNS) es una base de datos distribuida encargada de traducir nombres de dominio intuitivos a direcciones IP. Es uno de los sistemas más críticos de internet. Esto se facilita mediante una estructura jerárquica dividida en varias zonas, comenzando por la zona raíz de nivel superior.

Cada dominio puede usar distintos tipos de registros DNS. Estos son unos de los más usados: 

- **NS**: Name server records contain the name of the authoritative servers hosting the DNS records for a domain.
- **A**: Also known as a host record, the "_a record_" contains the IPv4 address of a hostname (such as www.megacorpone.com).
- **AAAA**: Also known as a quad A host record, the "_aaaa record_" contains the IPv6 address of a host name (such as www.megacorpone.com).
- **MX**: Mail Exchange records contain the names of the servers responsible for handling email for the domain. A domain can contain multiple MX records.
- **PTR**: Pointer Records are used in reverse lookup zones and can find the records associated with an IP address.
- **CNAME**: Canonical Name Records are used to create aliases for other host records.
- **TXT**: Text records can contain any arbitrary data and be used for various purposes, such as domain ownership verification.

Obtener información de lo que contiene un registro DNS es muy util cuando estamos obteniendo información de manera activa

Esta es la forma en la que podemos usar el comando **host** para obtener la dirección ip del donminio

```bash
host www.megacorpone.com

www.megacorpone.com has address 149.56.244.87
```

El comando host la mayoría de veces nos va a retornar un registro tipo **A**, pero si queremos obtener otros tipos de registro como el **MX** o el **TXT**, podemos usar la opción -t para especificar el tipo de registro que queremos buscar.

```bash
host -t mx megacorpone.com

megacorpone.com mail is handled by 50 mail.megacorpone.com.
megacorpone.com mail is handled by 10 fb.mail.gandi.net.
megacorpone.com mail is handled by 20 spool.mail.gandi.net.
megacorpone.com mail is handled by 60 mail2.megacorpone.com
```

En este caso, primero ejecutamos el comando host para recuperar únicamente los registros MX de megacorpone.com, lo que arrojó cuatro registros de servidor de correo diferentes. Cada servidor tiene una prioridad diferente (10, 20, 50, 60) y el servidor con la prioridad más baja se utilizará primero para reenviar el correo dirigido al dominio megacorpone.com (fb.mail.gandi.net).

Ahora ejecutemos para obtener los tipo **TXT**

``` bash
host -t txt megacorpone.com

megacorpone.com descriptive text "Try Harder"
megacorpone.com descriptive text "google-site-verification=U7B_b0HNeBtY4qYGQZNsEYXfCJ32hMNV3GtC0wWq5pA"
```

Podemos determinar si megacorpone.com  tiene un server con  el hostname "idontexist" usando el siguiente comando:

```bash
host idontexist.megacorpone.com

Host idontexist.megacorpone.com not found: 3(NXDOMAIN)
```

Los ataques de fuerza bruta son tecnicas basadas en prueba y error, para poder obtener información relevante debemos usar diferente tipo de combinaciones de comandos, opciones, usuario, contraseña, etc.

Podemos automatizar el ataque de dns usando un txt sencillo como librería de datos para darle uso con el comando host

``` bash
cat list.txt

www
ftp
mail
owa
proxy
router


for ip in $(cat list.txt); do host $ip.megacorpone.com; done

www.megacorpone.com has address 149.56.244.87
Host ftp.megacorpone.com not found: 3(NXDOMAIN)
mail.megacorpone.com has address 51.222.169.212
Host owa.megacorpone.com not found: 3(NXDOMAIN)
Host proxy.megacorpone.com not found: 3(NXDOMAIN)
router.megacorpone.com has address 51.222.169.214

```

Aquí obtuvimos información util con esta pequeña librería de ejemplo que creamos pero, para ser mas prácticos, podemos usar wordlist con cientos de datos del proyecto [_SecLists_](https://github.com/danielmiessler/SecLists) para poder usarlo en vez de nuestra pequeña librería. Para obtenerla usamos el comando `sudo apt install seclists` . Con la información obtenida podemos realizar reverse lookups para obtener el dominio relacionado con la ip.

```bash
for ip in $(seq 200 254); do host 51.222.169.$ip; done | grep -v "not found"

208.169.222.51.in-addr.arpa domain name pointer admin.megacorpone.com.
209.169.222.51.in-addr.arpa domain name pointer beta.megacorpone.com.
210.169.222.51.in-addr.arpa domain name pointer fs1.megacorpone.com.
211.169.222.51.in-addr.arpa domain name pointer intranet.megacorpone.com.
212.169.222.51.in-addr.arpa domain name pointer mail.megacorpone.com.
213.169.222.51.in-addr.arpa domain name pointer mail2.megacorpone.com.
214.169.222.51.in-addr.arpa domain name pointer router.megacorpone.com.
215.169.222.51.in-addr.arpa domain name pointer siem.megacorpone.com.
216.169.222.51.in-addr.arpa domain name pointer snmp.megacorpone.com.
217.169.222.51.in-addr.arpa domain name pointer syslog.megacorpone.com.
218.169.222.51.in-addr.arpa domain name pointer support.megacorpone.com.
219.169.222.51.in-addr.arpa domain name pointer test.megacorpone.com.
220.169.222.51.in-addr.arpa domain name pointer vpn.megacorpone.com
```


[_DNSRecon_](https://github.com/darkoperator/dnsrecon) es una herramienta completa escrita en python para la enumeración dns. Podemos usarla de la siguiente manera:

```bash
dnsrecon -d megacorpone.com -t std

[*] std: Performing General Enumeration against: megacorpone.com...
[-] DNSSEC is not configured for megacorpone.com
[*] 	 SOA ns1.megacorpone.com 51.79.37.18
[*] 	 NS ns2.megacorpone.com 51.222.39.63
[*] 	 Bind Version for 51.222.39.63 "9.18.24-1-Debian"
[*] 	 NS ns3.megacorpone.com 66.70.207.180
[*] 	 Bind Version for 66.70.207.180 "9.18.24-1-Debian"
[*] 	 NS ns1.megacorpone.com 51.79.37.18
[*] 	 Bind Version for 51.79.37.18 "9.18.24-1-Debian"
[*] 	 MX mail2.megacorpone.com 167.114.21.69
[*] 	 MX mail.megacorpone.com 167.114.21.68
[*] 	 MX fb.mail.gandi.net 217.70.178.215
[*] 	 MX fb.mail.gandi.net 217.70.178.217
[*] 	 MX fb.mail.gandi.net 217.70.178.216
[*] 	 MX spool.mail.gandi.net 217.70.178.1
[*] 	 MX fb.mail.gandi.net 2001:4b98:dc4:8::215
[*] 	 MX fb.mail.gandi.net 2001:4b98:dc4:8::216
[*] 	 MX fb.mail.gandi.net 2001:4b98:dc4:8::217
[*] 	 MX spool.mail.gandi.net 2001:4b98:e00::1
[*] 	 TXT megacorpone.com google-site-verification=U7B_b0HNeBtY4qYGQZNsEYXfCJ32hMNV3GtC0wWq5pA
[*] 	 TXT megacorpone.com Try Harder
[*] Enumerating SRV Records
[-] No SRV Records Found for megacorpone.com


```

También podemos combinarlo con el archivo que creamos para realizar un ataque dns

```bash
dnsrecon -d megacorpone.com -D ~/list.txt -t brt

[*] Using the dictionary file: /home/kali/list.txt (provided by user)
[*] brt: Performing host and subdomain brute force against megacorpone.com...
[+] 	 A www.megacorpone.com 149.56.244.87
[+] 	 A mail.megacorpone.com 51.222.169.212
[+] 	 A router.megacorpone.com 51.222.169.214
[+] 3 Records Found
```


[_DNSEnum_](https://www.kali.org/tools/dnsenum/) Es otra herramienta popular de enumeración DNS que permite automatizar aún más la enumeración DNS. Podemos pasarle algunas opciones y ser más precisos pero aquí solo lo usaré pasando el dominio.

```bash
dnsenum VERSION:1.3.1

-----   megacorpone.com   -----


Host's addresses:
__________________



Name Servers:
______________

ns1.megacorpone.com.                     62       IN    A        51.79.37.18
ns2.megacorpone.com.                     62       IN    A        51.222.39.63
ns3.megacorpone.com.                     62       IN    A        66.70.207.180


Mail (MX) Servers:
___________________

mail.megacorpone.com.                    300      IN    A        167.114.21.68
mail2.megacorpone.com.                   300      IN    A        167.114.21.69
spool.mail.gandi.net.                    365      IN    A        217.70.178.1
fb.mail.gandi.net.                       60       IN    A        217.70.178.215
fb.mail.gandi.net.                       60       IN    A        217.70.178.217
fb.mail.gandi.net.                       60       IN    A        217.70.178.216


Trying Zone Transfers and getting Bind Versions:
_________________________________________________


Trying Zone Transfer for megacorpone.com on ns2.megacorpone.com ... 
megacorpone.com.                         300      IN    SOA               (
megacorpone.com.                         300      IN    TXT            "Try
megacorpone.com.                         300      IN    TXT               (
megacorpone.com.                         300      IN    MX               10
megacorpone.com.                         300      IN    MX               20
megacorpone.com.                         300      IN    MX               50
megacorpone.com.                         300      IN    MX               60
megacorpone.com.                         300      IN    NS       ns1.megacorpone.com.
megacorpone.com.                         300      IN    NS       ns2.megacorpone.com.
megacorpone.com.                         300      IN    NS       ns3.megacorpone.com.
admin.megacorpone.com.                   300      IN    A        167.114.21.64
ai.megacorpone.com.                      300      IN    A        149.56.244.87
beta.megacorpone.com.                    300      IN    A        167.114.21.65
fs1.megacorpone.com.                     300      IN    A        167.114.21.66
intranet.megacorpone.com.                300      IN    A        167.114.21.67
mail.megacorpone.com.                    300      IN    A        167.114.21.68
mail2.megacorpone.com.                   300      IN    A        167.114.21.69
ns1.megacorpone.com.                     300      IN    A        51.79.37.18
ns2.megacorpone.com.                     300      IN    A        51.222.39.63
ns3.megacorpone.com.                     300      IN    A        66.70.207.180
router.megacorpone.com.                  300      IN    A        167.114.21.70
siem.megacorpone.com.                    300      IN    A        167.114.21.71
snmp.megacorpone.com.                    300      IN    A        167.114.21.72
support.megacorpone.com.                 300      IN    A        167.114.21.74
syslog.megacorpone.com.                  300      IN    A        167.114.21.73
test.megacorpone.com.                    300      IN    A        167.114.21.75
vpn.megacorpone.com.                     300      IN    A        167.114.21.76
vpn2.megacorpone.com.                    300      IN    A        167.114.21.77
vpndev.megacorpone.com.                  300      IN    A        167.114.21.78
vpnprod.megacorpone.com.                 300      IN    A        167.114.21.79
www.megacorpone.com.                     300      IN    A        149.56.244.87
www2.megacorpone.com.                    300      IN    A        149.56.244.87

Trying Zone Transfer for megacorpone.com on ns3.megacorpone.com ... 
AXFR record query failed: REFUSED

Trying Zone Transfer for megacorpone.com on ns1.megacorpone.com ... 
AXFR record query failed: REFUSED


Brute forcing with /usr/share/dnsenum/dns.txt:
_______________________________________________
```


Podemos conectarnos a maquinas windows usando el comando `xfreerdp`. La sintaxis es `/u:` el usuario, `/p:` la contraseña y `/v:` la direccion ip

```bash
xfreerdp /u:student /p:lab /v:192.168.50.152
```

Ya dentro podemos ejecutar un comando que nos resuelva el registro tipo A de el host que queramos.

```bash
nslookup mail.megacorptwo.com


DNS request timed out.
    timeout was 2 seconds.
Server:  UnKnown
Address:  192.168.50.151

Name:    mail.megacorptwo.com
Address:  192.168.50.154

```

nslookup de manera similar al comando host en linux nos permite granular la consulta. En este caso vamos a realizarla para registros DNS tipo TXT.

```bash
nslookup -type=TXT info.megacorptwo.com 192.168.50.151
Server:  UnKnown
Address:  192.168.50.151

info.megacorptwo.com    text =

        "greetings from the TXT record body
```

---

## 4.2. TCP/UDP Port Scanning Theory

El escaneo de puertos es el proceso de inspeccionar los puertos TCP o UDP en una máquina remota con la intención de detectar qué servicios se están ejecutando en el objetivo y qué vectores de ataque potenciales pueden existir.

TCP significa (Transmission Control Protocol), es un protocolo fundamental para la comunicación en redes de computadoras. Permite a los dispositivos intercambiar información de manera y confiable. TCP se asegura de que lleguen los paquetes, incluso si se producen errores, perdidas o latencias durante la transmisión

UDP significa User Datagram Protocol, es un protocolo de comunicación usado en redes de computadoras, es como TCP, la diferencia es que prioriza la velocidad y la baja latencia pero no asegura la llegada de los paquetes, no establece una conexión formal para enviar los datos (conexión formal hablando de three-way handshake).

Al momento de querer realizar un escaneo debemos pensar bastante bien que tipo de escaneo usaremos. Tenemos que tener en cuenta el alcance de nuestro escaneo, ya que el proceso incorrecto podría probar latencia al cliente y es algo que no queremos.
Para empezar podemos escanear los puertos 80 y 443 de nuestra lista de servidores.

Para escanear puertos podemos usar netcat, no esta hecho para eso pero lo podemos usar de manera rudimentaria. Para hacerlo vamos a usar el three-way-handshake, esto sucede cuando dos host intentan comunicarse entre si pueden negociar parámetros de red tcp para una conexión socket y transmitir datos.

En términos básicos, un host envía un paquete TCP SYN a un servidor en un puerto de destino. Si el puerto de destino está abierto, el servidor responde con un paquete SYN-ACK y el host cliente envía un paquete ACK para completar el protocolo de enlace. Si el protocolo de enlace se completa correctamente, el puerto se considera abierto.

Podemos demostrar esto ejecutando un escaneo TCP netcat en un rango de puertos 3388-3390.
Usaremos la opción -w para especificar el tiempo de espera de la conexión en segundos, así como -z para especificar el modo de E/S cero, que se utiliza para escanear y no envía datos.

```bash
nc -nvv -w 1 -z 192.168.50.152 3388-3390
(UNKNOWN) [192.168.50.152] 3390 (?) : Connection refused
(UNKNOWN) [192.168.50.152] 3389 (ms-wbt-server) open
(UNKNOWN) [192.168.50.152] 3388 (?) : Connection refused
 sent 0, rcvd 0
```

Como podemos ver el puerto 3389 está abierto, mientras que las conexiones del puerto 3388 y 3390 están cerradas. En esta captura de wireshark podremos ver esta transaccionalidad.

![[Pasted image 20250708214035.png]]

Ahora vamos con el UDP, este protocolo es más directo, no es tan laborioso con el apartado de la conexión como TCP. Vamos a correr el comando netcat con la opción -u que indica UDP, lo haremos del rango de puertos 120-123

```bash
nc -nv -u -z -w 1 192.168.50.149 120-123
(UNKNOWN) [192.168.50.149] 123 (ntp) open
```

Esta es la captura de wireshark que nos muestra como UDP usa diferentes mecanismos a los de TCP

![[Pasted image 20250708215050.png]]

Normalmente se omite el escaneo UDP en los vectores de ataque que usan los pentesters ya que los firewall y los router a veces arrojan paquetes ICMP, lo que conlleva a falsos positivos. Algunas veces suele ser util pero, normalmente se opta por el escaneo a puertos TCP

---

## 4.3. Port Scanning with Nmap

[_Nmap_](http://nmap.org/) (written by Gordon Lyon, aka Fyodor) es la herramienta de escaneo de puertos más popular que existe. Lleva en desarrollo más de dos decadas y sigue agregando funcionalidades para el escaneo de puertos.

Muchos de los ejemplos que se usan para escaneo de puertos usan `sudo`. Esto es porque algunas de las nuevas funcionalidades requieren acceso a [_raw sockets_](http://man7.org/linux/man-pages/man7/raw.7.html). Los Raw Sockets permiten la manipulación completa de paquetes TCP y UDP. Sin accesso a los Raw Sockets Nmap es limitado, ya que este recurre a la creación de paquetes mediante el estandar [_Berkeley socket API_](https://networkprogrammingnotes.blogspot.com/p/berkeley-sockets.html).

Antes de explorar algunos ejemplos de escaneo de puertos, debemos comprender el impacto que cada escaneo de Nmap deja en la red y los hosts escaneados.

Un escaneo TCP predeterminado de Nmap escaneará los 1000 puertos más populares de una máquina. Antes de ejecutar escaneos a ciegas, examinemos la cantidad de tráfico enviado por este tipo de escaneo. Escanearemos una de las máquinas del laboratorio mientras monitorizamos la cantidad de tráfico enviado al host de destino mediante [_iptabes_](http://netfilter.org/projects/iptables/index.html)

Usaremos varias opciones de iptables. Primero, usaremos la opción -I para insertar una nueva regla en una cadena dada, que en este caso incluye las cadenas INPUT (Entrante) y OUTPUT (Saliente), seguidas del número de regla. Podemos usar -s para especificar una dirección IP de origen, -d para especificar una dirección IP de destino y -j para aceptar el tráfico. Finalmente, usaremos la opción -Z para poner a cero los contadores de paquetes y bytes en todas las cadenas.

```bash
sudo iptables -I INPUT 1 -s 45.223.163.216 -j ACCEPT

sudo iptables -I OUTPUT 1 -d 45.223.163.216 -j ACCEPT

sudo iptables -Z
```

Ahora vamos a generar algo de trafico con nmap

```bash
nmap 45.223.163.216
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-10 13:28 EDT
Nmap scan report for 45.223.163.216
Host is up (0.079s latency).
Not shown: 602 filtered tcp ports (no-response)
PORT      STATE SERVICE
21/tcp    open  ftp
25/tcp    open  smtp
37/tcp    open  time
43/tcp    open  whois
53/tcp    open  domain
80/tcp    open  http
81/tcp    open  hosts2-ns
82/tcp    open  xfer
83/tcp    open  mit-ml-dev
84/tcp    open  ctf
85/tcp    open  mit-ml-dev
88/tcp    open  kerberos-sec
89/tcp    open  su-mit-tg
90/tcp    open  dnsix
99/tcp    open  metagram
100/tcp   open  newacct
110/tcp   open  pop3
119/tcp   open  nntp
143/tcp   open  imap
211/tcp   open  914c-g
212/tcp   open  anet
389/tcp   open  ldap
443/tcp   open  https
444/tcp   open  snpp
465/tcp   open  smtps
500/tcp   open  isakmp
543/tcp   open  klogin
554/tcp   open  rtsp
555/tcp   open  dsf
587/tcp   open  submission
631/tcp   open  ipp
636/tcp   open  ldapssl
777/tcp   open  multiling-http
783/tcp   open  spamassassin
800/tcp   open  mdbs_daemon
801/tcp   open  device
808/tcp   open  ccproxy-http
843/tcp   open  unknown
880/tcp   open  unknown
888/tcp   open  accessbuilder
900/tcp   open  omginitialrefs
990/tcp   open  ftps
993/tcp   open  imaps
995/tcp   open  pop3s
999/tcp   open  garcon
1000/tcp  open  cadlock
1001/tcp  open  webpush
1002/tcp  open  windows-icfw
1010/tcp  open  surf
1011/tcp  open  unknown
1024/tcp  open  kdm
1025/tcp  open  NFS-or-IIS
1028/tcp  open  unknown
1029/tcp  open  ms-lsa
1033/tcp  open  netinfo
1054/tcp  open  brvread
1056/tcp  open  vfo
1065/tcp  open  syscomlan
1066/tcp  open  fpo-fns
1067/tcp  open  instl_boots
1068/tcp  open  instl_bootc
1074/tcp  open  warmspotMgmt
1080/tcp  open  socks
1081/tcp  open  pvuniwien
1082/tcp  open  amt-esd-prot
1083/tcp  open  ansoft-lm-1
1108/tcp  open  ratio-adp
1111/tcp  open  lmsocialserver
1121/tcp  open  rmpp
1174/tcp  open  fnet-remote-ui
1175/tcp  open  dossier
1183/tcp  open  llsurfup-http
1185/tcp  open  catchpole
1186/tcp  open  mysql-cluster
1187/tcp  open  alias
1192/tcp  open  caids-sensor
1198/tcp  open  cajo-discovery
1199/tcp  open  dmidi
1201/tcp  open  nucleus-sand
1234/tcp  open  hotline
1352/tcp  open  lotusnotes
1433/tcp  open  ms-sql-s
1434/tcp  open  ms-sql-m
1443/tcp  open  ies-lm
1455/tcp  open  esl-lm
1494/tcp  open  citrix-ica
1500/tcp  open  vlsi-lm
1521/tcp  open  oracle
1700/tcp  open  mps-raft
1717/tcp  open  fj-hdnet
1720/tcp  open  h323q931
1805/tcp  open  enl-name
1935/tcp  open  rtmp
1971/tcp  open  netop-school
1972/tcp  open  intersys-cache
1974/tcp  open  drp
1984/tcp  open  bigbrother
2000/tcp  open  cisco-sccp
2001/tcp  open  dc
2006/tcp  open  invokator
2010/tcp  open  search
2020/tcp  open  xinupageserver
2022/tcp  open  down
2048/tcp  open  dls-monitor
2049/tcp  open  nfs
2065/tcp  open  dlsrpn
2068/tcp  open  avocentkvm
2099/tcp  open  h2250-annex-g
2100/tcp  open  amiganetfs
2103/tcp  open  zephyr-clt
2105/tcp  open  eklogin
2106/tcp  open  ekshell
2107/tcp  open  msmq-mgmt
2111/tcp  open  kx
2119/tcp  open  gsigatekeeper
2121/tcp  open  ccproxy-ftp
2126/tcp  open  pktcable-cops
2135/tcp  open  gris
2144/tcp  open  lv-ffx
2160/tcp  open  apc-2160
2161/tcp  open  apc-agent
2170/tcp  open  eyetv
2179/tcp  open  vmrdp
2190/tcp  open  tivoconnect
2196/tcp  open  unknown
2200/tcp  open  ici
2222/tcp  open  EtherNetIP-1
2260/tcp  open  apc-2260
2288/tcp  open  netml
2301/tcp  open  compaqdiag
2323/tcp  open  3d-nfsd
2381/tcp  open  compaq-https
2382/tcp  open  ms-olap3
2399/tcp  open  fmpro-fdal
2522/tcp  open  windb
2525/tcp  open  ms-v-worlds
2557/tcp  open  nicetec-mgmt
2602/tcp  open  ripd
2604/tcp  open  ospfd
2607/tcp  open  connection
2638/tcp  open  sybase
2701/tcp  open  sms-rcinfo
2809/tcp  open  corbaloc
2811/tcp  open  gsiftp
2910/tcp  open  tdaccess
2967/tcp  open  symantec-av
2968/tcp  open  enpp
2998/tcp  open  iss-realsec
3000/tcp  open  ppp
3001/tcp  open  nessus
3003/tcp  open  cgms
3005/tcp  open  deslogin
3006/tcp  open  deslogind
3011/tcp  open  trusted-web
3017/tcp  open  event_listener
3030/tcp  open  arepa-cas
3031/tcp  open  eppc
3052/tcp  open  powerchute
3071/tcp  open  csd-mgmt-port
3077/tcp  open  orbix-loc-ssl
3268/tcp  open  globalcatLDAP
3269/tcp  open  globalcatLDAPssl
3306/tcp  open  mysql
3333/tcp  open  dec-notes
3389/tcp  open  ms-wbt-server
3404/tcp  open  unknown
3551/tcp  open  apcupsd
3580/tcp  open  nati-svrloc
3690/tcp  open  svn
4000/tcp  open  remoteanything
4001/tcp  open  newoak
4002/tcp  open  mlchat-proxy
4003/tcp  open  pxc-splr-ft
4006/tcp  open  pxc-spvr
4343/tcp  open  unicall
4443/tcp  open  pharos
4445/tcp  open  upnotifyp
4446/tcp  open  n1-fwp
4449/tcp  open  privatewire
4567/tcp  open  tram
4848/tcp  open  appserv-http
4900/tcp  open  hfcs
5000/tcp  open  upnp
5001/tcp  open  commplex-link
5002/tcp  open  rfe
5003/tcp  open  filemaker
5004/tcp  open  avt-profile-1
5009/tcp  open  airport-admin
5050/tcp  open  mmcc
5051/tcp  open  ida-agent
5054/tcp  open  rlm-admin
5060/tcp  open  sip
5061/tcp  open  sip-tls
5080/tcp  open  onscreen
5100/tcp  open  admd
5120/tcp  open  barracuda-bbs
5200/tcp  open  targus-getdata
5222/tcp  open  xmpp-client
5225/tcp  open  hp-server
5226/tcp  open  hp-status
5269/tcp  open  xmpp-server
5280/tcp  open  xmpp-bosh
5440/tcp  open  unknown
5500/tcp  open  hotline
5544/tcp  open  unknown
5555/tcp  open  freeciv
5560/tcp  open  isqlplus
5566/tcp  open  westec-connect
5678/tcp  open  rrac
5800/tcp  open  vnc-http
5900/tcp  open  vnc
5901/tcp  open  vnc-1
5902/tcp  open  vnc-2
5903/tcp  open  vnc-3
5904/tcp  open  ag-swim
5906/tcp  open  rpas-c2
5907/tcp  open  dsd
5910/tcp  open  cm
5911/tcp  open  cpdlc
5915/tcp  open  unknown
5959/tcp  open  unknown
5985/tcp  open  wsman
5986/tcp  open  wsmans
5987/tcp  open  wbem-rmi
5988/tcp  open  wbem-http
5989/tcp  open  wbem-https
5998/tcp  open  ncd-diag
5999/tcp  open  ncd-conf
6000/tcp  open  X11
6001/tcp  open  X11:1
6002/tcp  open  X11:2
6003/tcp  open  X11:3
6004/tcp  open  X11:4
6005/tcp  open  X11:5
6006/tcp  open  X11:6
6007/tcp  open  X11:7
6009/tcp  open  X11:9
6100/tcp  open  synchronet-db
6510/tcp  open  mcer-port
6543/tcp  open  mythtv
6565/tcp  open  unknown
6580/tcp  open  parsec-master
6666/tcp  open  irc
6699/tcp  open  napster
6779/tcp  open  unknown
6788/tcp  open  smc-http
6789/tcp  open  ibm-db2-admin
7000/tcp  open  afs3-fileserver
7001/tcp  open  afs3-callback
7002/tcp  open  afs3-prserver
7004/tcp  open  afs3-kaserver
7007/tcp  open  afs3-bos
7019/tcp  open  doceri-ctl
7025/tcp  open  vmsvc-2
7070/tcp  open  realserver
7100/tcp  open  font-service
7106/tcp  open  unknown
7200/tcp  open  fodms
7201/tcp  open  dlip
7443/tcp  open  oracleas-https
7777/tcp  open  cbt
7778/tcp  open  interwise
7999/tcp  open  irdmi2
8000/tcp  open  http-alt
8001/tcp  open  vcom-tunnel
8002/tcp  open  teradataordbms
8007/tcp  open  ajp12
8008/tcp  open  http
8009/tcp  open  ajp13
8010/tcp  open  xmpp
8011/tcp  open  unknown
8021/tcp  open  ftp-proxy
8022/tcp  open  oa-system
8031/tcp  open  unknown
8042/tcp  open  fs-agent
8045/tcp  open  unknown
8080/tcp  open  http-proxy
8081/tcp  open  blackice-icecap
8082/tcp  open  blackice-alerts
8083/tcp  open  us-srv
8084/tcp  open  websnp
8085/tcp  open  unknown
8086/tcp  open  d-s-n
8087/tcp  open  simplifymedia
8088/tcp  open  radan-http
8089/tcp  open  unknown
8090/tcp  open  opsmessaging
8093/tcp  open  unknown
8099/tcp  open  unknown
8100/tcp  open  xprint-server
8180/tcp  open  unknown
8181/tcp  open  intermapper
8192/tcp  open  sophos
8193/tcp  open  sophos
8194/tcp  open  sophos
8200/tcp  open  trivnet1
8222/tcp  open  unknown
8290/tcp  open  unknown
8300/tcp  open  tmi
8333/tcp  open  bitcoin
8383/tcp  open  m2mservices
8400/tcp  open  cvd
8402/tcp  open  abarsd
8443/tcp  open  https-alt
8500/tcp  open  fmtp
8600/tcp  open  asterix
8701/tcp  open  unknown
8800/tcp  open  sunwebadmin
8873/tcp  open  dxspider
8888/tcp  open  sun-answerbook
8899/tcp  open  ospf-lite
9000/tcp  open  cslistener
9001/tcp  open  tor-orport
9002/tcp  open  dynamid
9003/tcp  open  unknown
9009/tcp  open  pichat
9010/tcp  open  sdr
9011/tcp  open  d-star
9040/tcp  open  tor-trans
9050/tcp  open  tor-socks
9071/tcp  open  unknown
9080/tcp  open  glrpc
9081/tcp  open  cisco-aqos
9090/tcp  open  zeus-admin
9091/tcp  open  xmltec-xmlmail
9099/tcp  open  unknown
9100/tcp  open  jetdirect
9101/tcp  open  jetdirect
9102/tcp  open  jetdirect
9103/tcp  open  jetdirect
9110/tcp  open  unknown
9111/tcp  open  DragonIDSConsole
9200/tcp  open  wap-wsp
9207/tcp  open  wap-vcal-s
9220/tcp  open  unknown
9500/tcp  open  ismserver
9502/tcp  open  unknown
9503/tcp  open  unknown
9876/tcp  open  sd
9900/tcp  open  iua
9943/tcp  open  unknown
9998/tcp  open  distinct32
9999/tcp  open  abyss
10000/tcp open  snet-sensor-mgmt
10001/tcp open  scp-config
10002/tcp open  documentum
10003/tcp open  documentum_s
10004/tcp open  emcrmirccd
10009/tcp open  swdtp-sv
10010/tcp open  rxapi
10012/tcp open  unknown
10024/tcp open  unknown
10025/tcp open  unknown
10082/tcp open  amandaidx
11110/tcp open  sgi-soap
12000/tcp open  cce4x
12174/tcp open  unknown
12265/tcp open  unknown
12345/tcp open  netbus
13456/tcp open  unknown
13722/tcp open  netbackup
13782/tcp open  netbackup
13783/tcp open  netbackup
14000/tcp open  scotty-ft
14238/tcp open  unknown
14441/tcp open  unknown
14442/tcp open  unknown
15000/tcp open  hydap
15002/tcp open  onep-tls
15003/tcp open  unknown
15004/tcp open  unknown
15660/tcp open  bex-xr
15742/tcp open  unknown
16000/tcp open  fmsas
16001/tcp open  fmsascon
16012/tcp open  unknown
16016/tcp open  unknown
16018/tcp open  unknown
16080/tcp open  osxwebadmin
18040/tcp open  unknown
18101/tcp open  unknown
19101/tcp open  unknown
20000/tcp open  dnp
27000/tcp open  flexlm0
30000/tcp open  ndmps
31337/tcp open  Elite
40193/tcp open  unknown
50000/tcp open  ibm-db2
50001/tcp open  unknown
50002/tcp open  iiimsf
50003/tcp open  unknown
50006/tcp open  unknown
50300/tcp open  unknown
50500/tcp open  unknown
50800/tcp open  unknown
55055/tcp open  unknown
55555/tcp open  unknown
60443/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 6.06 seconds
```


Ahora, revisemos algunas estadísticas de iptables para tener una idea más clara del tráfico generado por nuestro escaneo. Podemos usar la opción -v para añadir más detalle a la salida, -n para habilitar la salida numérica y -L para listar las reglas presentes en todas las cadenas.

```bash
sudo iptables -vn -L
Chain INPUT (policy ACCEPT 283 packets, 64760 bytes)
 pkts bytes target     prot opt in     out     source               destination         
 2040 89760 ACCEPT     all  --  *      *       45.223.163.216       0.0.0.0/0          

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 285 packets, 130K bytes)
 pkts bytes target     prot opt in     out     source               destination         
 4257  179K ACCEPT     all  --  *      *       0.0.0.0/0            45.223.163.216 
```

Como podemos ver hemos dejado una huella de trafico de 179KB.

Ahora vamos a dejar iptables a cero. A partir de ahí vamos a escanear todos los puertos de nuestro objetivo, usaremos la opción -p para especificar todos los puertos.

Esta prueba es ficticia :v

```bash

sudo iptables -Z

nmap -p 1-65535 192.168.50.149
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-09 05:23 EST
Nmap scan report for 192.168.50.149
Host is up (0.11s latency).
Not shown: 65510 closed tcp ports (conn-refused)
PORT      STATE SERVICE
53/tcp    open  domain
88/tcp    open  kerberos-sec
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
389/tcp   open  ldap
445/tcp   open  microsoft-ds
464/tcp   open  kpasswd5
593/tcp   open  http-rpc-epmap
636/tcp   open  ldapssl
3268/tcp  open  globalcatLDAP
3269/tcp  open  globalcatLDAPssl
5985/tcp  open  wsman
9389/tcp  open  adws
47001/tcp open  winrm
49664/tcp open  unknown
...

Nmap done: 1 IP address (1 host up) scanned in 2141.22 seconds

sudo iptables -vn -L
Chain INPUT (policy ACCEPT 67996 packets, 6253K bytes)
 pkts bytes target     prot opt in     out     source               destination
68724 2749K ACCEPT     all  --  *      *       192.168.50.149      0.0.0.0/0

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination

Chain OUTPUT (policy ACCEPT 67923 packets, 7606K bytes)
 pkts bytes target     prot opt in     out     source               destination
68807 4127K ACCEPT     all  --  *      *       0.0.0.0/0            192.168.50.149
```


Un escaneo local completo de los 65 535 puertos generó unos 4 MB de tráfico, mucho más que el escaneo TCP por defecto, pero reveló más puertos abiertos. Extrapolando, escanear con Nmap una red clase C completa (254 hosts) podría consumir más de 1 GB de tráfico, por lo que al evaluar redes grandes (clase A o B) conviene equilibrar la exhaustividad con las limitaciones de ancho de banda.

Herramientas modernas como MASSCAN y RustScan escanean más rápido que Nmap, pero disparan muchos paquetes simultáneos; Nmap, en cambio, aplica límites de velocidad que reducen la congestión y lo vuelven más sigiloso.

El nivel de discreción requerido depende de los objetivos: en pentesting el énfasis suele estar en encontrar vulnerabilidades, mientras que en red teaming se busca medir la capacidad del SOC para detectar intrusos, por lo que la furtividad es crucial.

Tras revisar el uso básico de Nmap, se presentarán técnicas de escaneo, empezando por el escaneo SYN (“stealth”), la opción predeterminada cuando se dispone de privilegios de sockets sin especificar otra modalidad. 

El escaneo SYN es una técnica de escaneo de puertos TCP que consiste en enviar paquetes SYN a distintos puertos del objetivo sin completar el "three-way handshake". Si un puerto está abierto, el host responde con un SYN-ACK, lo que indica su disponibilidad. El escáner, entonces, no envía el ACK final, evitando así establecer una conexión completa. Esto permite detectar puertos abiertos de forma rápida y relativamente sigilosa.

```bash
sudo nmap -sS 192.168.50.149

Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-09 06:31 EST
Nmap scan report for 192.168.50.149
Host is up (0.11s latency).
Not shown: 989 closed tcp ports (reset)
PORT     STATE SERVICE
53/tcp   open  domain
88/tcp   open  kerberos-sec
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
389/tcp  open  ldap
445/tcp  open  microsoft-ds
464/tcp  open  kpasswd5
593/tcp  open  http-rpc-epmap
636/tcp  open  ldapssl
3268/tcp open  globalcatLDAP
3269/tcp open  globalcatLDAPssl
```

Como el escaneo SYN no completa el _three-way handshake_, la conexión no llega a la capa de aplicación, por lo que no queda registrada en los logs de esa capa. Además, es más rápido y eficiente, ya que implica menos tráfico.

El término “stealth” se usaba porque los firewalls antiguos no registraban conexiones TCP incompletas. Sin embargo, los firewalls modernos sí lo hacen, por lo que el nombre puede inducir a error.

El siguiente método que veremos es el escaneo TCP Connect. Este sí realiza la conexión completa (los tres pasos del handshake). Nmap lo usa por defecto cuando el usuario no tiene privilegios para usar sockets sin procesar (_raw sockets_), ya que emplea la API de sockets de Berkeley, que no requiere permisos elevados.

El escaneo TCP Connect es más lento que el SYN scan porque Nmap debe esperar a que se complete la conexión para saber el estado del puerto. Aun así, puede ser útil en situaciones específicas, como cuando se escanea a través de ciertos proxies. Para usarlo explícitamente, se emplea la opción `-sT`.

```bash
nmap -sT 192.168.50.149
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-09 06:44 EST
Nmap scan report for 192.168.50.149
Host is up (0.11s latency).
Not shown: 989 closed tcp ports (conn-refused)
PORT     STATE SERVICE
53/tcp   open  domain
88/tcp   open  kerberos-sec
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
389/tcp  open  ldap
445/tcp  open  microsoft-ds
464/tcp  open  kpasswd5
593/tcp  open  http-rpc-epmap
636/tcp  open  ldapssl
3268/tcp open  globalcatLDAP
3269/tcp open  globalcatLDAPssl
...
```

El resultado del escaneo TCP Connect reveló algunos servicios abiertos que suelen estar activos solo en sistemas Windows, especialmente en controladores de dominio (Domain Controllers), lo que ya nos permite inferir tanto el sistema operativo como el posible rol del host, incluso con un escaneo simple.

Ahora, pasamos al escaneo UDP con Nmap. Este utiliza dos métodos para determinar si un puerto UDP está abierto o cerrado:

1. **Método ICMP "port unreachable"**: se envía un paquete vacío al puerto; si se recibe un mensaje ICMP indicando que el puerto no es accesible, se considera cerrado.
    
2. **Paquetes específicos por protocolo**: para puertos conocidos como el 161 (usado por SNMP), Nmap envía un paquete SNMP válido, esperando una respuesta de la aplicación que escuche en ese puerto.
    

Para realizar un [_UDP scan_](https://nmap.org/book/scan-methods-udp-scan.html), se usa la opción `-sU`, y es necesario ejecutarlo con `sudo` ya que se requieren privilegios para usar _raw sockets_.

```bash
sudo nmap -sU 192.168.50.149
Starting Nmap 7.70 ( https://nmap.org ) at 2019-03-04 11:46 EST
Nmap scan report for 192.168.131.149
Host is up (0.11s latency).
Not shown: 977 closed udp ports (port-unreach)
PORT      STATE         SERVICE
123/udp   open          ntp
389/udp   open          ldap
...
Nmap done: 1 IP address (1 host up) scanned in 22.49 seconds
```

El escaneo UDP (-sU) también se puede utilizar junto con un escaneo TCP SYN (-sS) para construir una imagen más completa de nuestro objetivo.

```bash
sudo nmap -sU -sS 192.168.50.149

Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-09 08:16 EST
Nmap scan report for 192.168.50.149
Host is up (0.10s latency).
Not shown: 989 closed tcp ports (reset), 977 closed udp ports (port-unreach)
PORT      STATE         SERVICE
53/tcp    open          domain
88/tcp    open          kerberos-sec
135/tcp   open          msrpc
139/tcp   open          netbios-ssn
389/tcp   open          ldap
445/tcp   open          microsoft-ds
464/tcp   open          kpasswd5
593/tcp   open          http-rpc-epmap
636/tcp   open          ldapssl
3268/tcp  open          globalcatLDAP
3269/tcp  open          globalcatLDAPssl
53/udp    open          domain
123/udp   open          ntp
389/udp   open          ldap
```


El escaneo conjunto TCP y UDP permitió identificar puertos UDP abiertos adicionales, revelando más servicios activos en el host objetivo.

Ahora podemos aplicar lo aprendido a un rango completo de red mediante **Network Sweeping** --> también conocido como barrido de ping o barrido ICMP, es una ==técnica utilizada para identificar dispositivos activos en una red enviando solicitudes ICMP (Peticiones de eco) a un rango de direcciones IP==.

Este enfoque es útil para manejar grandes volúmenes de hosts o reducir el tráfico de red. Consiste en realizar escaneos amplios iniciales para identificar hosts activos y luego ejecutar escaneos más específicos solo sobre ellos.

En Nmap, el barrido de red se realiza con la opción `-sn`. Este proceso de descubrimiento de hosts no se limita a enviar peticiones ICMP Echo; también incluye:

- Un paquete TCP SYN al puerto 443
    
- Un paquete TCP ACK al puerto 80
    
- Una solicitud ICMP Timestamp
    

Estas sondas múltiples mejoran la fiabilidad al determinar si un host está disponible.

```bash
nmap -sn 192.168.50.1-253

Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 03:19 EST
Nmap scan report for 192.168.50.6
Host is up (0.12s latency).
Nmap scan report for 192.168.50.8
Host is up (0.12s latency).
...
Nmap done: 254 IP addresses (13 hosts up) scanned in 3.74 seconds
```

Buscar máquinas activas con el comando grep en una salida estándar de nmap puede ser engorroso. En su lugar, usemos el parámetro de salida "greppable" de Nmap, -oG, para guardar estos resultados en un formato más manejable.

```bash
nmap -v -sn 192.168.50.1-253 -oG ping-sweep.txt
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 03:21 EST
Initiating Ping Scan at 03:21
...
Read data files from: /usr/bin/../share/nmap
Nmap done: 254 IP addresses (13 hosts up) scanned in 3.74 seconds
...

grep Up ping-sweep.txt | cut -d " " -f 2
192.168.50.6
192.168.50.8
192.168.50.9
...
```

También podemos rastrear puertos TCP o UDP específicos en la red, buscando servicios y puertos comunes para localizar sistemas que puedan ser útiles o que presenten vulnerabilidades conocidas. Este análisis suele ser más preciso que un barrido de ping.

```bash
nmap -p 80 192.168.50.1-253 -oG web-sweep.txt
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 03:50 EST
Nmap scan report for 192.168.50.6
Host is up (0.11s latency).

PORT   STATE SERVICE
80/tcp open  http

Nmap scan report for 192.168.50.8
Host is up (0.11s latency).

PORT   STATE  SERVICE
80/tcp closed http
...

kali@kali:~$ grep open web-sweep.txt | cut -d" " -f2
192.168.50.6
192.168.50.20
192.168.50.21
```

Para ahorrar tiempo y recursos de red, también podemos escanear varias IP y buscar una lista corta de puertos comunes. Por ejemplo, realicemos un escaneo de conexión TCP de los 20 puertos TCP principales con la opción --top-ports y habilitemos la detección de la versión del sistema operativo, el escaneo de scripts y el traceroute con -A.

```bash
nmap -sT -A --top-ports=20 192.168.50.1-253 -oG top-port-sweep.txt
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 04:04 EST
Nmap scan report for 192.168.50.6
Host is up (0.12s latency).

PORT     STATE  SERVICE       VERSION
21/tcp   closed ftp
22/tcp   open   ssh           OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 56:57:11:b5:dc:f1:13:d3:50:88:b8:ab:a9:83:e2:29 (RSA)
|   256 4f:1d:f2:55:cb:40:e0:76:b4:36:90:19:a2:ba:f0:44 (ECDSA)
|_  256 67:46:b3:97:26:a9:e3:a8:4d:eb:20:b3:9b:8d:7a:32 (ED25519)
23/tcp   closed telnet
25/tcp   closed smtp
53/tcp   closed domain
80/tcp   open   http          Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Under Construction
110/tcp  closed pop3
111/tcp  closed rpcbind
```


Los 20 puertos principales que Nmap escanea por defecto se determinan a partir del archivo `/usr/share/nmap/nmap-services`. Este archivo usa un formato simple de tres columnas separadas por espacios:

1. **Nombre del servicio**
    
2. **Número de puerto y protocolo** (por ejemplo, `80/tcp`)
    
3. **Frecuencia del puerto**
    

Todo lo que aparece después de la tercera columna se ignora, aunque suele incluir comentarios marcados con el símbolo `#`.

La **"[_port frequency_](https://nmap.org/book/nmap-services.html)".** indica con qué frecuencia ese puerto fue encontrado abierto en escaneos periódicos de internet realizados por Nmap, lo que permite priorizar los puertos más comúnmente utilizados.

```bash
cat /usr/share/nmap/nmap-services 
...
finger    79/udp    0.000956
http    80/sctp    0.000000    # www-http | www | World Wide Web HTTP
http    80/tcp    0.484143    # World Wide Web HTTP
http    80/udp    0.035767    # World Wide Web HTTP
hosts2-ns    81/tcp    0.012056    # HOSTS2 Name Server
hosts2-ns    81/udp    0.001005    # HOSTS2 Name Server
```


En este punto, podemos realizar un escaneo más exhaustivo sobre máquinas individuales que presenten muchos servicios o parezcan especialmente interesantes.

Existen múltiples formas creativas de escanear para **ahorrar ancho de banda** o **reducir nuestra visibilidad**, así como técnicas de descubrimiento de hosts que vale la pena investigar más a fondo.

Ya hemos escaneado hosts que muestran algunos servicios, lo que nos permite **inferir el sistema operativo** del objetivo. Afortunadamente, Nmap incluye una funcionalidad llamada **OS Fingerprinting**.

Esta se activa con la opción `-O` y permite a Nmap **adivinar el sistema operativo** del objetivo analizando los paquetes que devuelve. Esto es posible porque los distintos sistemas operativos implementan la pila TCP/IP de forma ligeramente distinta (por ejemplo, usan diferentes valores por defecto de TTL o tamaños de ventana TCP), lo que genera un "fingerprint" único que Nmap intenta emparejar con su base de datos.

Nmap solo mostrará un sistema operativo si el _fingerprint_ coincide con alta precisión. Si queremos ver una suposición aproximada, podemos usar la opción `--osscan-guess`, que forzará a Nmap a mostrar el sistema operativo estimado, aunque la coincidencia no sea exacta.

A continuación, se podría realizar un escaneo básico de fingerprinting con Nmap usando estas opciones.

```bash
sudo nmap -O 192.168.50.14 --osscan-guess
...
Running (JUST GUESSING): Microsoft Windows 2008|2012|2016|7|Vista (88%)
OS CPE: cpe:/o:microsoft:windows_server_2008::sp1 cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_server_2012:r2 cpe:/o:microsoft:windows_server_2016 cpe:/o:microsoft:windows_7 cpe:/o:microsoft:windows_vista::sp1:home_premium
Aggressive OS guesses: Microsoft Windows Server 2008 SP1 or Windows Server 2008 R2 (88%), Microsoft Windows Server 2012 or Windows Server 2012 R2 (88%), Microsoft Windows Server 2012 R2 (88%), Microsoft Windows Server 2012 (87%), Microsoft Windows Server 2016 (87%), Microsoft Windows 7 (86%), Microsoft Windows Vista Home Premium SP1 (85%), Microsoft Windows 7 Professional (85%)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
...
```

La respuesta del escaneo sugiere que el sistema operativo del objetivo es probablemente **Windows 2008 R2, 2012, 2016, Vista o Windows 7**.

Es importante tener en cuenta que el **OS Fingerprinting no siempre es 100 % preciso**, ya que dispositivos intermedios como firewalls o proxies pueden **modificar los encabezados de los paquetes**, afectando los resultados.

Una vez identificado el sistema operativo base, podemos profundizar aún más identificando **los servicios específicos que corren en cada puerto**. Para ello, se puede usar la opción `-A`, que permite a Nmap **inspeccionar banners de servicios** y además ejecutar **scripts de enumeración de sistema operativo y servicios** sobre el objetivo. Esta técnica ofrece una visión más detallada del entorno que estamos analizando.

```bash
nmap -sT -A 192.168.50.14
Nmap scan report for 192.168.50.14
Host is up (0.12s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT    STATE SERVICE       VERSION
21/tcp  open  ftp?
| fingerprint-strings:
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, GenericLines, NULL, RPCCheck, SSLSessionReq, TLSSessionReq, TerminalServerCookie:
|     220-FileZilla Server 1.2.0
|     Please visit https://filezilla-project.org/
|   GetRequest:
|     220-FileZilla Server 1.2.0
|     Please visit https://filezilla-project.org/
|     What are you trying to do? Go away.
|   HTTPOptions, RTSPRequest:
|     220-FileZilla Server 1.2.0
|     Please visit https://filezilla-project.org/
|     Wrong command.
|   Help:
|     220-FileZilla Server 1.2.0
|     Please visit https://filezilla-project.org/
|     214-The following commands are recognized.
|     USER TYPE SYST SIZE RNTO RNFR RMD REST QUIT
|     HELP XMKD MLST MKD EPSV XCWD NOOP AUTH OPTS DELE
|     CDUP APPE STOR ALLO RETR PWD FEAT CLNT MFMT
|     MODE XRMD PROT ADAT ABOR XPWD MDTM LIST MLSD PBSZ
|     NLST EPRT PASS STRU PASV STAT PORT
|_    Help ok.
| ftp-syst:
|_  SYST: UNIX emulated by FileZilla.
| ssl-cert: Subject: commonName=filezilla-server self signed certificate
| Not valid before: 2022-01-06T15:37:24
|_Not valid after:  2023-01-07T15:42:24
|_ssl-date: TLS randomness does not represent time
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?
Nmap done: 1 IP address (1 host up) scanned in 55.67 seconds
```

En el ejemplo anterior usamos la opción `-A` para ejecutar un escaneo de servicios con opciones avanzadas. Sin embargo, si solo queremos hacer un escaneo **básico de servicios**, basta con usar la opción `-sV`.

El _banner grabbing_ —es decir, recolectar información que los servicios publican al conectarse— **aumenta el tráfico generado y puede ralentizar el escaneo**, por lo que debemos tener cuidado con las opciones que usamos en Nmap y su impacto.

Además, los administradores pueden **modificar los banners** y establecer nombres de servicios falsos para **engañar a posibles atacantes**.

Ahora que ya cubrimos las funciones principales de Nmap, nos centraremos en los **scripts del Nmap Scripting Engine (NSE)**.

Con NSE, podemos **automatizar tareas de escaneo** mediante scripts creados por la comunidad o personalizados. Estos scripts permiten desde **enumeración DNS**, ataques de **fuerza bruta**, hasta **detección de vulnerabilidades**. Los scripts se encuentran en el directorio:  
`/usr/share/nmap/scripts`.

Por ejemplo, el script `http-headers` se conecta al servicio HTTP de un objetivo y **detecta los encabezados soportados**, lo que puede revelar información útil sobre configuraciones del servidor web.

```bash
nmap --script http-headers 192.168.50.6
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 13:53 EST
Nmap scan report for 192.168.50.6
Host is up (0.14s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
| http-headers:
|   Date: Thu, 10 Mar 2022 18:53:29 GMT
|   Server: Apache/2.4.41 (Ubuntu)
|   Last-Modified: Thu, 10 Mar 2022 18:51:54 GMT
|   ETag: "d1-5d9e1b5371420"
|   Accept-Ranges: bytes
|   Content-Length: 209
|   Vary: Accept-Encoding
|   Connection: close
|   Content-Type: text/html
|
|_  (Request type: HEAD)

Nmap done: 1 IP address (1 host up) scanned in 5.11 seconds
```

Para ver más información sobre un script, podemos utilizar la opción --script-help, que muestra una descripción del script y una URL donde podemos encontrar información más detallada, como los argumentos del script y ejemplos de uso.

```bash
nmap --script-help http-headers
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 13:54 EST

http-headers
Categories: discovery safe
https://nmap.org/nsedoc/scripts/http-headers.html
  Performs a HEAD request for the root folder ("/") of a web server and displays the HTTP headers returned.
...
```

Cuando no se tiene acceso a internet, mucha de la información útil sobre los scripts NSE también se puede consultar directamente dentro de los archivos de script en el sistema.

Vale la pena explorar los diferentes **scripts NSE**, ya que muchos son muy útiles y ahorran tiempo en tareas comunes.

Tras haber aprendido a realizar escaneos de puertos desde Kali, ahora veremos cómo aplicar esos mismos conceptos desde un **host con Windows**.

Si estamos haciendo una enumeración inicial de red desde un **portátil con Windows sin acceso a internet**, no podremos instalar herramientas adicionales como la versión de Nmap para Windows. En ese caso, debemos recurrir a la estrategia de **“living off the land”**, es decir, usar herramientas que ya vienen integradas en el sistema.

Afortunadamente, **PowerShell** incluye funciones que pueden ser de ayuda. Una de ellas es [_Test-NetConnection_](https://docs.microsoft.com/en-us/powershell/module/nettcpip/test-netconnection?view=windowsserver2022-ps), que permite comprobar si una IP responde a ICMP (ping) y si un puerto TCP específico está abierto en el host de destino.

Por ejemplo, en un cliente con Windows 11, podríamos comprobar si el puerto SMB (445) está abierto en un controlador de dominio con el siguiente comando:****

```
PS C:\Users\student> Test-NetConnection -Port 445 192.168.50.151

ComputerName     : 192.168.50.151
RemoteAddress    : 192.168.50.151
RemotePort       : 445
InterfaceAlias   : Ethernet0
SourceAddress    : 192.168.50.152
TcpTestSucceeded : True
```

El valor devuelto en el parámetro `TcpTestSucceeded` indica si el **puerto 445 está abierto**.

Podemos automatizar este proceso para escanear los primeros **1024 puertos** del controlador de dominio con un _one-liner_ de PowerShell como el siguiente:

```
1..1024 | % {echo ((New-Object Net.Sockets.TcpClient).Connect("192.168.50.151", $_)) "TCP port $_ is open"} 2>$null
```

Esto hace lo siguiente:

- Se crea un rango del 1 al 1024, que se pasa a un bucle `foreach`.
    
- La variable `$_` representa el número de puerto actual.
    
- Para cada puerto, se crea un objeto `Net.Sockets.TcpClient` y se intenta establecer una conexión TCP contra la IP objetivo en ese puerto.
    
- Si la conexión se logra, se imprime un mensaje indicando que ese puerto está abierto.
    
- La salida de errores (`2>$null`) se descarta para que solo se muestren los puertos abiertos.
    

Este enfoque demuestra cómo **PowerShell puede replicar funcionalidades básicas de Nmap** incluso en entornos restringidos sin herramientas externas. Y aunque este es solo el punto de partida, PowerShell puede ampliarse mucho más para cubrir tareas similares a las que realiza Nmap tradicionalmente.

### Ejercicio Captura la bandera

Escaneamos un rango de puertos tcp con nmap

```bash
sudo nmap -p 50000-60000 -sS 192.168.124.52
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-10 21:50 EDT
Nmap scan report for 192.168.124.52
Host is up (0.12s latency).
Not shown: 10000 closed tcp ports (reset)
PORT      STATE SERVICE
59811/tcp open  unknown
```

Luego con netcat (nc) nos conectamos a el para capturar la flag
```bash
nc 192.168.124.52 59811

You found me. Great job!
Here is your flag:
OS{33bf40278bfdb26b9fffc969c76b3658}
```


### Ejercicio paginas web
El **Nmap Scripting Engine (NSE)** tiene muchos scripts útiles para reconocimiento activo, incluyendo más de 100 para descubrimiento. En este reto, deberás usar un script NSE para encontrar el **título de las páginas web (HTTP title)** de los hosts con servidores web en la red del laboratorio. Esto te ayuda a identificar detalles como el propósito del sitio, versiones de software o páginas de login. Tu objetivo es encontrar el servidor web cuyo título sea **"Under Construction"**. La **flag está en el archivo `index.html`** de ese servidor.

```bash
nmap -p 80,443 --script=http-title 192.168.124.1-253 

Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-10 22:18 EDT
Nmap scan report for 192.168.124.6
Host is up (0.12s latency).

PORT    STATE  SERVICE
80/tcp  open   http
|_http-title: Under Construction
443/tcp closed https

Nmap scan report for 192.168.124.8
Host is up (0.12s latency).

PORT    STATE  SERVICE
80/tcp  closed http
443/tcp closed https

Nmap scan report for 192.168.124.9
Host is up (0.12s latency).

PORT    STATE  SERVICE
80/tcp  closed http
443/tcp closed https

Nmap scan report for 192.168.124.11
Host is up (0.12s latency).

PORT    STATE  SERVICE
80/tcp  closed http
443/tcp closed https

Nmap scan report for 192.168.124.12
Host is up (0.12s latency).

PORT    STATE  SERVICE
80/tcp  closed http
443/tcp closed https

Nmap scan report for 192.168.124.13
Host is up (0.12s latency).

PORT    STATE  SERVICE
80/tcp  closed http
443/tcp open   https
|_http-title: Site doesn't have a title (text/html).

Nmap scan report for 192.168.124.14
Host is up (0.12s latency).

PORT    STATE  SERVICE
80/tcp  open   http
|_http-title: IIS Windows Server
443/tcp closed https

Nmap scan report for 192.168.124.15
Host is up (0.12s latency).

PORT    STATE  SERVICE
80/tcp  closed http
443/tcp closed https

Nmap scan report for 192.168.124.17
Host is up (0.12s latency).

PORT    STATE    SERVICE
80/tcp  filtered http
443/tcp filtered https

Nmap scan report for 192.168.124.20
Host is up (0.13s latency).

PORT    STATE  SERVICE
80/tcp  open   http
| http-title: LOGIN |Messaging Web Application
|_Requested resource was ./login.php
443/tcp closed https

Nmap scan report for 192.168.124.21
Host is up (0.13s latency).

PORT    STATE  SERVICE
80/tcp  open   http
|_http-title: Chicken monitoring site
443/tcp closed https

Nmap scan report for 192.168.124.22
Host is up (0.12s latency).

PORT    STATE  SERVICE
80/tcp  closed http
443/tcp closed https

Nmap scan report for 192.168.124.23
Host is up (0.12s latency).

PORT    STATE    SERVICE
80/tcp  filtered http
443/tcp filtered https

Nmap scan report for 192.168.124.149
Host is up (0.12s latency).

PORT    STATE  SERVICE
80/tcp  closed http
443/tcp closed https

Nmap scan report for 192.168.124.151
Host is up (0.12s latency).

PORT    STATE  SERVICE
80/tcp  closed http
443/tcp closed https

Nmap scan report for 192.168.124.152
Host is up (0.12s latency).

PORT    STATE  SERVICE
80/tcp  closed http
443/tcp closed https

Nmap done: 253 IP addresses (16 hosts up) scanned in 8.29 seconds
```


Hacemos curl para obtener la flag
```bash
curl http://192.168.124.6
<html>
	<head>
		<title>Under Construction</title>
	</head>
	<body>
                Flag: OS{923db2528d36557d8134cccf468873f9}
		This site is still under construction, please come back later.
	</body>
</html>
```


---

## ## 4.4. SMB Enumeration

El protocolo **SMB (Server Message Block)** ha tenido un historial de seguridad deficiente durante muchos años, debido a su implementación compleja y su naturaleza abierta. Desde sesiones SMB nulas no autenticadas en Windows 2000 y XP hasta múltiples vulnerabilidades, SMB ha enfrentado numerosos problemas.

No obstante, el protocolo ha sido **actualizado y mejorado** en paralelo con las versiones de Windows.

El servicio **NetBIOS** escucha en el puerto TCP 139 y en varios puertos UDP. Es importante señalar que **SMB (puerto TCP 445) y NetBIOS son protocolos distintos**. NetBIOS es un protocolo de capa de sesión independiente que permite la comunicación entre equipos en una red local. Aunque SMB moderno puede funcionar sin NetBIOS, **NBT (NetBIOS sobre TCP/IP)** aún se requiere por razones de compatibilidad y suelen estar habilitados juntos. Por ello, la enumeración de ambos servicios suele realizarse en conjunto, utilizando herramientas como **nmap**.

```bash
nmap -v -p 139,445 -oG smb.txt 192.168.50.1-254

kali@kali:~$ cat smb.txt
# Nmap 7.92 scan initiated Thu Mar 17 06:03:12 2022 as: nmap -v -p 139,445 -oG smb.txt 192.168.50.1-254
# Ports scanned: TCP(2;139,445) UDP(0;) SCTP(0;) PROTOCOLS(0;)
Host: 192.168.50.1 ()	Status: Down
...
Host: 192.168.50.21 ()	Status: Up
Host: 192.168.50.21 ()	Ports: 139/closed/tcp//netbios-ssn///, 445/closed/tcp//microsoft-ds///
...
Host: 192.168.50.217 ()	Status: Up
Host: 192.168.50.217 ()	Ports: 139/closed/tcp//netbios-ssn///, 445/closed/tcp//microsoft-ds///
# Nmap done at Thu Mar 17 06:03:18 2022 -- 254 IP addresses (15 hosts up) scanned in 6.17 seconds
```

Aquí guardamos la salida del escaneo en el archivo smb.txt, el cual nos ha rebelado que los puertos 139 y 445 están abiertos.

Existe otra herramienta que está especializada en identificar la información de **NetBIOS**, se llama ``nbscan``. Podemos usar esto para consultar el servicio de nombres NetBIOS en busca de nombres NetBIOS válidos, especificando el puerto UDP de origen como 137 con la opción -r.

```bash
sudo nbtscan -r 192.168.50.0/24
Doing NBT name scan for addresses from 192.168.50.0/24

IP address       NetBIOS Name     Server    User             MAC address
------------------------------------------------------------------------------
192.168.50.124   SAMBA            <server>  SAMBA            00:00:00:00:00:00
192.168.50.134   SAMBAWEB         <server>  SAMBAWEB         00:00:00:00:00:00
```


El escaneo reveló **dos nombres NetBIOS correspondientes a dos hosts**. Este tipo de información puede ser muy útil para **mejorar el contexto** sobre los sistemas escaneados, ya que los nombres NetBIOS suelen ser **descriptivos del rol del host** dentro de la organización. Estos datos pueden alimentar el ciclo de recopilación de información y conducir a **nuevas revelaciones**.

Además, **Nmap ofrece múltiples scripts NSE** que pueden utilizarse para **descubrir y enumerar servicios SMB**. Estos scripts se encuentran en el directorio:  
`/usr/share/nmap/scripts`.

```bash
ls -1 /usr/share/nmap/scripts/smb*

/usr/share/nmap/scripts/smb2-capabilities.nse
/usr/share/nmap/scripts/smb2-security-mode.nse
/usr/share/nmap/scripts/smb2-time.nse
/usr/share/nmap/scripts/smb2-vuln-uptime.nse
/usr/share/nmap/scripts/smb-brute.nse
/usr/share/nmap/scripts/smb-double-pulsar-backdoor.nse
/usr/share/nmap/scripts/smb-enum-domains.nse
/usr/share/nmap/scripts/smb-enum-groups.nse
/usr/share/nmap/scripts/smb-enum-processes.nse
/usr/share/nmap/scripts/smb-enum-sessions.nse
/usr/share/nmap/scripts/smb-enum-shares.nse
/usr/share/nmap/scripts/smb-enum-users.nse
/usr/share/nmap/scripts/smb-os-discovery.nse
```

Se identificaron **varios scripts NSE de Nmap relacionados con SMB** que permiten realizar tareas como la **detección del sistema operativo y la enumeración mediante SMB**.

El script de **descubrimiento SMB** solo funciona si el **SMBv1 está habilitado** en el objetivo, lo cual **no es común en versiones modernas de Windows**. Sin embargo, **muchos sistemas heredados aún usan SMBv1**, y en este caso se ha activado esta versión en el host con Windows para simular ese escenario.

Probemos el módulo **`smb-os-discovery`** en un cliente con **Windows 11**.

```bash
nmap -v -p 139,445 --script smb-os-discovery 192.168.50.152
...
PORT    STATE SERVICE      REASON
139/tcp open  netbios-ssn  syn-ack
445/tcp open  microsoft-ds syn-ack

Host script results:
| smb-os-discovery:
|   OS: Windows 10 Pro 22000 (Windows 10 Pro 6.3)
|   OS CPE: cpe:/o:microsoft:windows_10::-
|   Computer name: client01
|   NetBIOS computer name: CLIENT01\x00
|   Domain name: megacorptwo.com
|   Forest name: megacorptwo.com
|   FQDN: client01.megacorptwo.com
|_  System time: 2022-03-17T11:54:20-07:00
...
```

El script en cuestión identificó un posible sistema operativo del host, pero **el resultado fue incorrecto**, ya que el equipo objetivo ejecuta **Windows 11**, no Windows 10 como se indicó.

Como se mencionó antes, los resultados de **enumeración de servicios y sistemas operativos con Nmap deben tomarse con precaución**, ya que los algoritmos no son completamente precisos.

A diferencia de la huella del sistema operativo que ofrece Nmap por defecto, **la enumeración vía scripts NSE** puede aportar **información adicional**, como el **dominio** y otros datos relacionados con **Active Directory**. Además, este método **genera menos tráfico**, lo que permite que **pase desapercibido en redes empresariales**.

Luego de cubrir la enumeración SMB con Kali Linux, se abordará **cómo hacerlo desde un cliente Windows**.

Una herramienta útil para ello es **`net view`**, que permite **listar dominios, recursos y equipos** asociados a un host. Por ejemplo, al conectarse desde la máquina cliente `client01`, se pueden listar todos los recursos compartidos en `dc01`.

```bash
net view \\dc01 /all
Shared resources at \\dc01

Share name  Type  Used as  Comment

-------------------------------------------------------------------------------
ADMIN$      Disk           Remote Admin
C$          Disk           Default share
IPC$        IPC            Remote IPC
NETLOGON    Disk           Logon server share
SYSVOL      Disk           Logon server share
The command completed successfully.
```


Al proporcionar la palabra clave ``/all``, podemos enumerar las acciones administrativas que terminan con el signo de dólar.

### Ejercicio de practica

Debemos escanear un rango se ips que tengan abierto el puerto SMB y luego con enum4linux debemos obtener la flag que tiene el usuario alfred.

Obtener las ips con los puertos 139 y 445 abiertos

```bash
nmap -v -p 139,445 -oG smb.txt 192.168.140.1-254

cat smb.txt | grep Up

Host: 192.168.140.6 ()	Status: Up
Host: 192.168.140.8 ()	Status: Up
Host: 192.168.140.9 ()	Status: Up
Host: 192.168.140.11 ()	Status: Up
Host: 192.168.140.12 ()	Status: Up
Host: 192.168.140.13 ()	Status: Up
Host: 192.168.140.14 ()	Status: Up
Host: 192.168.140.15 ()	Status: Up
Host: 192.168.140.17 ()	Status: Up
Host: 192.168.140.20 ()	Status: Up
Host: 192.168.140.21 ()	Status: Up
Host: 192.168.140.22 ()	Status: Up
Host: 192.168.140.23 ()	Status: Up
Host: 192.168.140.149 ()	Status: Up
Host: 192.168.140.151 ()	Status: Up
Host: 192.168.140.152 ()	Status: Up
Host: 192.168.140.254 ()	Status: Up

```

Usando el comando de enum con cada una terminamos hallando que la ip terminada en .13 tiene el username alfred. Así que podemos tomar la bandera del inicio y listo

```bash
enum4linux -a 192.168.140.13
Starting enum4linux v0.9.1 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sat Jul 12 13:01:15 2025

 =========================================( Target Information )=========================================

Target ........... 192.168.140.13
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ===========================( Enumerating Workgroup/Domain on 192.168.140.13 )===========================


[+] Got domain/workgroup name: WORKGROUP


 ===============================( Nbtstat Information for 192.168.140.13 )===============================

Looking up status of 192.168.140.13
	SAMBA           <00> -         B <ACTIVE>  Workstation Service
	SAMBA           <03> -         B <ACTIVE>  Messenger Service
	SAMBA           <20> -         B <ACTIVE>  File Server Service
	WORKGROUP       <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
	WORKGROUP       <1e> - <GROUP> B <ACTIVE>  Browser Service Elections

	MAC Address = 00-00-00-00-00-00

 ==================================( Session Check on 192.168.140.13 )==================================


[+] Server 192.168.140.13 allows sessions using username '', password ''


 ===============================( Getting domain SID for 192.168.140.13 )===============================

Domain Name: WORKGROUP
Domain Sid: (NULL SID)

[+] Can't determine if host is part of domain or part of a workgroup


 ==================================( OS information on 192.168.140.13 )==================================


[E] Can't get OS info with smbclient


[+] Got OS info for 192.168.140.13 from srvinfo: 
	SAMBA          Wk Sv PrQ Unx NT SNT samba server (Samba, Ubuntu)
	platform_id     :	500
	os version      :	6.1
	server type     :	0x809a03


 ======================================( Users on 192.168.140.13 )======================================

Use of uninitialized value $users in print at ./enum4linux.pl line 972.
Use of uninitialized value $users in pattern match (m//) at ./enum4linux.pl line 975.

Use of uninitialized value $users in print at ./enum4linux.pl line 986.
Use of uninitialized value $users in pattern match (m//) at ./enum4linux.pl line 988.

 ================================( Share Enumeration on 192.168.140.13 )================================

smbXcli_negprot_smb1_done: No compatible protocol selected by server.

	Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	files           Disk      Flag: OS{3af1f9e74715063f6400e04a4552e26d}
	IPC$            IPC       IPC Service (samba server (Samba, Ubuntu))
Reconnecting with SMB1 for workgroup listing.
Protocol negotiation to server 192.168.140.13 (for a protocol between LANMAN1 and NT1) failed: NT_STATUS_INVALID_NETWORK_RESPONSE
Unable to connect with SMB1 -- no workgroup available

[+] Attempting to map shares on 192.168.140.13

//192.168.140.13/print$	Mapping: DENIED Listing: N/A Writing: N/A
//192.168.140.13/files	Mapping: OK Listing: OK Writing: N/A

[E] Can't understand response:

NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*
//192.168.140.13/IPC$	Mapping: N/A Listing: N/A Writing: N/A

 ===========================( Password Policy Information for 192.168.140.13 )===========================



[+] Attaching to 192.168.140.13 using a NULL share

[+] Trying protocol 139/SMB...

[+] Found domain(s):

	[+] SAMBA
	[+] Builtin

[+] Password Info for Domain: SAMBA

	[+] Minimum password length: 5
	[+] Password history length: None
	[+] Maximum password age: 37 days 6 hours 21 minutes 
	[+] Password Complexity Flags: 000000

		[+] Domain Refuse Password Change: 0
		[+] Domain Password Store Cleartext: 0
		[+] Domain Password Lockout Admins: 0
		[+] Domain Password No Clear Change: 0
		[+] Domain Password No Anon Change: 0
		[+] Domain Password Complex: 0

	[+] Minimum password age: None
	[+] Reset Account Lockout Counter: 30 minutes 
	[+] Locked Account Duration: 30 minutes 
	[+] Account Lockout Threshold: None
	[+] Forced Log off Time: 37 days 6 hours 21 minutes 



[+] Retieved partial password policy with rpcclient:


Password Complexity: Disabled
Minimum Password Length: 5


 ======================================( Groups on 192.168.140.13 )======================================


[+] Getting builtin groups:


[+]  Getting builtin group memberships:


[+]  Getting local groups:


[+]  Getting local group memberships:


[+]  Getting domain groups:


[+]  Getting domain group memberships:


 =================( Users on 192.168.140.13 via RID cycling (RIDS: 500-550,1000-1050) )=================


[I] Found new SID: 
S-1-22-1

[I] Found new SID: 
S-1-5-32

[I] Found new SID: 
S-1-5-32

[I] Found new SID: 
S-1-5-32

[I] Found new SID: 
S-1-5-32

[+] Enumerating users using SID S-1-22-1 and logon username '', password ''

S-1-22-1-1000 Unix User\alfred (Local User)

[+] Enumerating users using SID S-1-5-21-4030004202-475240355-4120303355 and logon username '', password ''

S-1-5-21-4030004202-475240355-4120303355-501 SAMBA\nobody (Local User)
S-1-5-21-4030004202-475240355-4120303355-513 SAMBA\None (Domain Group)

[+] Enumerating users using SID S-1-5-32 and logon username '', password ''

S-1-5-32-544 BUILTIN\Administrators (Local Group)
S-1-5-32-545 BUILTIN\Users (Local Group)
S-1-5-32-546 BUILTIN\Guests (Local Group)
S-1-5-32-547 BUILTIN\Power Users (Local Group)
S-1-5-32-548 BUILTIN\Account Operators (Local Group)
S-1-5-32-549 BUILTIN\Server Operators (Local Group)
S-1-5-32-550 BUILTIN\Print Operators (Local Group)

 ==============================( Getting printer info for 192.168.140.13 )==============================

No printers returned.


enum4linux complete on Sat Jul 12 13:09:56 2025

```


---

## 4.5 SMTP Enumeration

Los servidores de correo pueden ser utilizados como vectores de recolección de información durante una prueba de penetración. En particular, el protocolo SMTP (Simple Mail Transfer Protocol) incluye comandos que pueden revelar datos sensibles si no están adecuadamente restringidos.

#### Comandos relevantes:

- **VRFY**: Permite verificar si una dirección de correo electrónico existe en el servidor. Si el servidor responde afirmativamente, confirma la existencia del usuario.
    
- **EXPN**: Solicita la expansión de una lista de correo. El servidor devuelve los miembros de dicha lista, revelando potencialmente múltiples direcciones de correo internas.
    

#### Aplicación en pruebas de penetración:

Estos comandos pueden ser utilizados para:

- Enumerar usuarios válidos en un sistema.
    
- Obtener información sobre la estructura de correo interno.
    
- Apoyar técnicas de ingeniería social al identificar posibles objetivos dentro de una organización.
    

#### Riesgos de seguridad:
Cuando los servidores SMTP están mal configurados y permiten estos comandos sin restricciones, se convierten en fuentes de información valiosa para un atacante. La enumeración de usuarios es una fase crítica del reconocimiento en un ataque dirigido.

```bash
nc -nv 192.168.50.8 25
(UNKNOWN) [192.168.50.8] 25 (smtp) open
220 mail ESMTP Postfix (Ubuntu)
VRFY root
252 2.0.0 root
VRFY idontexist
550 5.1.1 <idontexist>: Recipient address rejected: User unknown in local recipient table
^C
```


Se puede utilizar la herramienta Netcat para conectarse a un servidor SMTP y enviar comandos manualmente, como `VRFY`, con el fin de verificar si existen ciertos usuarios. Las respuestas del servidor permiten distinguir entre usuarios válidos e inválidos. Por ejemplo, el código 252 indica que el servidor acepta el mensaje sin confirmar la existencia del usuario, mientras que el código 550 señala que el usuario no existe. Esta técnica puede emplearse para automatizar la enumeración de usuarios en un sistema.


```python
#!/usr/bin/python

import socket
import sys

if len(sys.argv) != 3:
        print("Usage: vrfy.py <username> <target_ip>")
        sys.exit(0)

# Create a Socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Connect to the Server
ip = sys.argv[2]
connect = s.connect((ip,25))

# Receive the banner
banner = s.recv(1024)

print(banner)

# VRFY a user
user = (sys.argv[1]).encode()
s.send(b'VRFY ' + user + b'\r\n')
result = s.recv(1024)

print(result)

# Close the socket
s.close()
```

Podemos ejecutar el script proporcionando el nombre de usuario que se va a probar como primer argumento y la IP de destino como segundo argumento.

```bash
python3 smtp.py root 192.168.50.8
b'220 mail ESMTP Postfix (Ubuntu)\r\n'
b'252 2.0.0 root\r\n'

python3 smtp.py johndoe 192.168.50.8
b'220 mail ESMTP Postfix (Ubuntu)\r\n'
b'550 5.1.1 <johndoe>: Recipient address rejected: User unknown in local recipient table\r\n'
```

De manera similar, podemos obtener información SMTP sobre nuestro objetivo desde el cliente de Windows 11

```
PS C:\Users\student> Test-NetConnection -Port 25 192.168.50.8

ComputerName     : 192.168.50.8
RemoteAddress    : 192.168.50.8
RemotePort       : 25
InterfaceAlias   : Ethernet0
SourceAddress    : 192.168.50.152
TcpTestSucceeded : True
```

Lamentablemente, Test-NetConnection no nos permite interactuar completamente con el servicio SMTP. Sin embargo, si aún no está habilitado, podemos instalar la versión de Microsoft del cliente Telnet:

```
PS C:\Windows\system32> dism /online /Enable-Feature /FeatureName:TelnetClient 
```

We should note that installing Telnet requires administrative privileges, which could present challenges if we are running as a low-privilege user. However, we could grab the Telnet binary located on another development machine of ours at `c:\windows\system32\telnet.exe` and transfer it to the Windows machine we are testing from.

Once we have enabled Telnet on the testing machine, we can connect to the target machine and perform enumeration as we did from Kali.

```
C:\Windows\system32>telnet 192.168.50.8 25
220 mail ESMTP Postfix (Ubuntu)
VRFY goofy
550 5.1.1 <goofy>: Recipient address rejected: User unknown in local recipient table
VRFY root
252 2.0.0 root
```

El resultado anterior muestra otro ejemplo de enumeración que podemos realizar desde un host de Windows comprometido cuando Kali no está disponible.

---

## 4.6 SNMP Enumeration

El **protocolo SNMP (Simple Network Management Protocol)**, diseñado para la administración de redes, suele estar mal configurado debido al desconocimiento por parte de muchos administradores. Esto lo convierte en una fuente frecuente de **filtración de información sensible** durante pruebas de penetración.

### Vulnerabilidades y debilidades del protocolo:

- **Basado en UDP**, por lo que es vulnerable a ataques de **IP spoofing** y **replay**.
    
- **SNMPv1, v2 y v2c** no cifran el tráfico ni las credenciales, lo que permite que un atacante intercepte información en redes locales.
    
- Usualmente se dejan configuradas las **community strings** por defecto: `public` y `private`.
    
- La autenticación es débil y fácilmente explotable.
    
- Aunque **SNMPv3** introduce autenticación y cifrado, inicialmente solo soportaba **DES-56**, considerado inseguro. Las versiones más nuevas ya permiten **AES-256**.
    

### Riesgo operativo:

Como SNMP está diseñado para gestionar dispositivos de red, una mala configuración puede comprometer toda una infraestructura. Un caso real describe cómo se descubrieron **credenciales SNMP reutilizadas** en una red corporativa con miles de routers Cisco. A través de esas credenciales privadas, se accedió y copiaron las configuraciones de todos los dispositivos, comprometiendo tanto la empresa como sus clientes. Esto demuestra cómo SNMP puede convertirse en un vector de ataque de gran impacto cuando se gestiona mal.

### MIB (Management Information Base):

El MIB es una base de datos jerárquica que organiza la información accesible a través de SNMP. Funciona como un árbol, donde las ramas representan funciones o fabricantes, y las hojas son variables que pueden consultarse o manipularse.

Algunos valores del MIB en entornos Windows permiten acceso a datos muy sensibles, como:

- Procesos del sistema
    
- Programas en ejecución
    
- Rutas de procesos
    
- Unidades de almacenamiento
    
- Usuarios del sistema
    
- Puertos TCP locales
    

Estos datos pueden obtenerse si el SNMP está mal configurado y se conoce la community string correspondiente.

Ejemplo de valores MIB en sistemas Windows:

| MIB OID                | Información accedida          |
| ---------------------- | ----------------------------- |
| 1.3.6.1.2.1.25.1.6.0   | Procesos del sistema          |
| 1.3.6.1.2.1.25.4.2.1.2 | Programas en ejecución        |
| 1.3.6.1.2.1.25.4.2.1.4 | Ruta de los procesos          |
| 1.3.6.1.2.1.25.2.3.1.4 | Unidades de almacenamiento    |
| 1.3.6.1.2.1.25.6.3.1.2 | Nombres de software instalado |
| 1.3.6.1.4.1.77.1.2.25  | Cuentas de usuario            |
| 1.3.6.1.2.1.6.13.1.3   | Puertos TCP locales           |

### Herramientas:

Para descubrir dispositivos con SNMP habilitado, se puede usar `nmap` con escaneo UDP:

```bash
sudo nmap -sU --open -p 161 192.168.50.1-254 -oG open-snmp.txt
Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-14 06:02 EDT
Nmap scan report for 192.168.50.151
Host is up (0.10s latency).

PORT    STATE SERVICE
161/udp open  snmp

Nmap done: 1 IP address (1 host up) scanned in 0.49 seconds
```


También es posible utilizar herramientas especializadas como **onesixtyone** para realizar ataques de fuerza bruta contra dispositivos SNMP. Esta herramienta prueba múltiples **community strings** sobre una lista de direcciones IP, con el objetivo de identificar aquellas que permiten el acceso.

Para usarla, es necesario preparar dos archivos de texto:

1. Una lista de **community strings** (por ejemplo, `public`, `private`, etc.).
    
2. Una lista de **direcciones IP** que se desea escanear.
    

Esta técnica permite automatizar la enumeración de dispositivos mal configurados que utilizan SNMP, especialmente útil en redes grandes durante una auditoría o prueba de penetración.

```bash
echo public > community
echo private >> community
echo manager >> community

for ip in $(seq 1 254); do echo 192.168.50.$ip; done > ips

onesixtyone -c community -i ips
Scanning 254 hosts, 3 communities
192.168.50.151 [public] Hardware: Intel64 Family 6 Model 79 Stepping 1 AT/AT COMPATIBLE - Software: Windows Version 6.3 (Build 17763 Multiprocessor Free)
```

Una vez que se identifican servicios SNMP activos, es posible comenzar a consultarlos para obtener datos específicos del MIB que puedan ser de interés.

Para interrogar y consultar estos valores, se puede utilizar una herramienta como **snmpwalk**, siempre que se conozca la **cadena de comunidad de solo lectura** de SNMP, que por defecto suele ser **"public"**.

Utilizando valores MIB específicos, se pueden enumerar los valores correspondientes del dispositivo. Por ejemplo, en un entorno de laboratorio con una máquina Windows que tiene expuesto un puerto SNMP y usa "public" como cadena de comunidad, se puede ejecutar un comando que recorra todo el árbol MIB. Este comando utiliza:

- `-c` para definir la cadena de comunidad,
    
- `-v` para indicar la versión de SNMP,
    
- `-t 10` para establecer un tiempo de espera de 10 segundos.

```bash
snmpwalk -c public -v1 -t 10 192.168.50.151
iso.3.6.1.2.1.1.1.0 = STRING: "Hardware: Intel64 Family 6 Model 79 Stepping 1 AT/AT COMPATIBLE - Software: Windows Version 6.3 (Build 17763 Multiprocessor Free)"
iso.3.6.1.2.1.1.2.0 = OID: iso.3.6.1.4.1.311.1.1.3.1.3
iso.3.6.1.2.1.1.3.0 = Timeticks: (78235) 0:13:02.35
iso.3.6.1.2.1.1.4.0 = STRING: "admin@megacorptwo.com"
iso.3.6.1.2.1.1.5.0 = STRING: "dc01.megacorptwo.com"
iso.3.6.1.2.1.1.6.0 = ""
iso.3.6.1.2.1.1.7.0 = INTEGER: 79
iso.3.6.1.2.1.2.1.0 = INTEGER: 24
```

Nuestro comando consultó un subárbol MIB específico asignado a todos los nombres de cuentas de usuario locales.

Como otro ejemplo, podemos enumerar todos los procesos en ejecución:

```bash
snmpwalk -c public -v1 192.168.50.151 1.3.6.1.2.1.25.4.2.1.2
iso.3.6.1.2.1.25.4.2.1.2.1 = STRING: "System Idle Process"
iso.3.6.1.2.1.25.4.2.1.2.4 = STRING: "System"
iso.3.6.1.2.1.25.4.2.1.2.88 = STRING: "Registry"
iso.3.6.1.2.1.25.4.2.1.2.260 = STRING: "smss.exe"
iso.3.6.1.2.1.25.4.2.1.2.316 = STRING: "svchost.exe"
iso.3.6.1.2.1.25.4.2.1.2.372 = STRING: "csrss.exe"
iso.3.6.1.2.1.25.4.2.1.2.472 = STRING: "svchost.exe"
iso.3.6.1.2.1.25.4.2.1.2.476 = STRING: "wininit.exe"
iso.3.6.1.2.1.25.4.2.1.2.484 = STRING: "csrss.exe"
iso.3.6.1.2.1.25.4.2.1.2.540 = STRING: "winlogon.exe"
iso.3.6.1.2.1.25.4.2.1.2.616 = STRING: "services.exe"
iso.3.6.1.2.1.25.4.2.1.2.632 = STRING: "lsass.exe"
iso.3.6.1.2.1.25.4.2.1.2.680 = STRING: "svchost.exe"
```

El comando SNMP ejecutado anteriormente devolvió un **arreglo de cadenas**, donde **cada una representa el nombre de un proceso que se está ejecutando** en el sistema remoto.

Esta información puede ser de **gran valor en un análisis de seguridad**, ya que:

- Puede revelar **aplicaciones vulnerables** que estén en ejecución.
    
- Permite identificar **software de seguridad activo**, como antivirus o firewalls, lo cual puede ser útil para evaluar medidas de defensa o planificar vectores de evasión.

De igual forma podemos consultar todo el software que está instalado en la máquina:

```bash
snmpwalk -c public -v1 192.168.50.151 1.3.6.1.2.1.25.6.3.1.2
iso.3.6.1.2.1.25.6.3.1.2.1 = STRING: "Microsoft Visual C++ 2019 X64 Minimum Runtime - 14.27.29016"
iso.3.6.1.2.1.25.6.3.1.2.2 = STRING: "VMware Tools"
iso.3.6.1.2.1.25.6.3.1.2.3 = STRING: "Microsoft Visual C++ 2019 X64 Additional Runtime - 14.27.29016"
iso.3.6.1.2.1.25.6.3.1.2.4 = STRING: "Microsoft Visual C++ 2015-2019 Redistributable (x86) - 14.27.290"
iso.3.6.1.2.1.25.6.3.1.2.5 = STRING: "Microsoft Visual C++ 2015-2019 Redistributable (x64) - 14.27.290"
iso.3.6.1.2.1.25.6.3.1.2.6 = STRING: "Microsoft Visual C++ 2019 X86 Additional Runtime - 14.27.29016"
iso.3.6.1.2.1.25.6.3.1.2.7 = STRING: "Microsoft Visual C++ 2019 X86 Minimum Runtime - 14.27.29016"
```

Al combinar esta información con la lista de procesos en ejecución obtenida anteriormente, puede resultar extremadamente valiosa para verificar la versión exacta del software que un proceso se está ejecutando en el host de destino.

Otra técnica de enumeración SNMP consiste en listar todos los puertos de escucha TCP actuales:

```bash
snmpwalk -c public -v1 192.168.50.151 1.3.6.1.2.1.6.13.1.3
iso.3.6.1.2.1.6.13.1.3.0.0.0.0.88.0.0.0.0.0 = INTEGER: 88
iso.3.6.1.2.1.6.13.1.3.0.0.0.0.135.0.0.0.0.0 = INTEGER: 135
iso.3.6.1.2.1.6.13.1.3.0.0.0.0.389.0.0.0.0.0 = INTEGER: 389
iso.3.6.1.2.1.6.13.1.3.0.0.0.0.445.0.0.0.0.0 = INTEGER: 445
iso.3.6.1.2.1.6.13.1.3.0.0.0.0.464.0.0.0.0.0 = INTEGER: 464
iso.3.6.1.2.1.6.13.1.3.0.0.0.0.593.0.0.0.0.0 = INTEGER: 593
iso.3.6.1.2.1.6.13.1.3.0.0.0.0.636.0.0.0.0.0 = INTEGER: 636
iso.3.6.1.2.1.6.13.1.3.0.0.0.0.3268.0.0.0.0.0 = INTEGER: 3268
iso.3.6.1.2.1.6.13.1.3.0.0.0.0.3269.0.0.0.0.0 = INTEGER: 3269
iso.3.6.1.2.1.6.13.1.3.0.0.0.0.5357.0.0.0.0.0 = INTEGER: 5357
iso.3.6.1.2.1.6.13.1.3.0.0.0.0.5985.0.0.0.0.0 = INTEGER: 5985
```

El valor entero obtenido en la salida representa los **puertos TCP en escucha** en el objetivo. Esta información es muy útil, ya que puede revelar **puertos que solo están escuchando localmente** y, por lo tanto, **descubrir servicios previamente desconocidos**.

---

## 5.1. Enumeración activa asistida por LLM

Una de las tareas fundamentales durante la fase de reconocimiento de una infraestructura es la **enumeración de subdominios DNS**. Y un paso crítico para que esta fase sea efectiva es construir una **wordlist robusta y enfocada**.

Tradicionalmente, esto implicaba revisar manualmente información pública sobre la empresa objetivo, inferir patrones de nombres y construir listas a partir de ello. Hoy, gracias al uso de **modelos de lenguaje (LLMs)** como ChatGPT, este proceso puede automatizarse de forma precisa y eficiente, ahorrando tiempo y generando mejores resultados.

---

### Generación de la wordlist con un LLM

Supongamos que el objetivo es `megacorpone.com`. Podemos utilizar un LLM para obtener información pública de la empresa (desde su sitio web, nombres de productos, departamentos, servicios, etc.) y, a partir de eso, generar una lista de subdominios probables.

Por ejemplo, se puede dar al modelo una instrucción como esta:

> **Usando datos públicos del sitio web de MegacorpOne y cualquier información que se pueda inferir sobre su estructura organizacional, productos o servicios, genera una lista completa de nombres de subdominios potenciales.**
> 
> - Incorpora patrones comunes de subdominios, como:
>     
>     - Términos relacionados a infraestructura (por ejemplo: `api`, `dev`, `test`, `staging`)
>         
>     - Términos de servicios (por ejemplo: `mail`, `auth`, `cdn`, `status`)
>         
>     - Términos funcionales o departamentales (por ejemplo: `hr`, `sales`, `support`)
>         
>     - Términos regionales (por ejemplo: `us`, `eu`, `asia`)
>         
>     - Considera normas comunes de la industria y términos relevantes para el sector de MegacorpOne.
>         
> 
> **Finalmente, compila los términos generados en una wordlist estructurada de 1000 palabras, optimizada para fuerza bruta de subdominios contra megacorpone.com.**
> 
> Asegúrate de que la salida esté en formato limpio, en minúsculas, sin duplicados, sin viñetas, y lista para copiar y pegar.  
> **La lista debe contener exactamente 1000 entradas únicas.**

> _Listing 67 - LLM's prompt to generate a DNS subdomain wordlist_

---

El modelo puede generar una salida extensa y con explicaciones adicionales, pero lo importante es enfocarse en el contenido útil: **la wordlist final de 1000 entradas únicas**.

La respuesta típica de un LLM será algo como:

> _He generado una wordlist estructurada de 1000 subdominios, optimizada para ataques de fuerza bruta sobre megacorpone.com. Puedes descargar el archivo directamente:_

> **Download Subdomain Wordlist**

> _Listing 68 - LLM's response with the DNS subdomain wordlist_

En este caso, el archivo viene adjunto y puede descargarse directamente a la máquina Kali para guardarlo como `wordlist.txt`. En caso de que la lista esté incluida directamente en el texto, simplemente se puede copiar y pegar.

Es importante recordar que las respuestas de los LLMs pueden variar. A veces devuelven listas más cortas, por lo que conviene repetir la petición o ajustar el prompt si no se obtiene lo que se necesita. La **persistencia es clave** incluso con herramientas automatizadas como esta.

---

### Ejecución del ataque con Gobuster

Una vez obtenida nuestra wordlist personalizada, podemos usar **Gobuster**, una herramienta rápida y flexible para hacer fuerza bruta de subdominios.

Primero, se instala en Kali Linux:

```bash
sudo apt update
sudo apt install gobuster
``` 

Con Gobuster instalado, lanzamos el ataque con el siguiente comando:

```bash
gobuster dns -d megacorpone.com -w wordlist.txt -t 10
``` 

Este comando hace lo siguiente:

- `dns`: activa el modo de enumeración DNS.
    
- `-d megacorpone.com`: define el dominio objetivo.
    
- `-w wordlist.txt`: indica la wordlist a utilizar (la generada por el LLM).
    
- `-t 10`: utiliza 10 hilos en paralelo para acelerar el proceso.
    

La salida se verá como:

```bash
gobuster dns -d megacorpone.com -w wordlist.txt -t 10
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Domain:     megacorpone.com
[+] Threads:    10
[+] Timeout:    1s
[+] Wordlist:   wordlist
===============================================================
Starting gobuster in DNS enumeration mode
===============================================================
[INFO] [-] Unable to validate base domain: megacorpone.com (lookup megacorpone.com on 8.8.8.8:53: no such host)
Found: admin.megacorpone.com
Found: router.megacorpone.com
Found: mail.megacorpone.com
Found: support.megacorpone.com
Found: test.megacorpone.com
```

Como se observa, el ataque reveló subdominios válidos como `admin.megacorpone.com`, `mail.megacorpone.com`, `support.megacorpone.com`, entre otros, lo cual valida la efectividad de una wordlist personalizada.

Como profesionales de seguridad ofensiva, está claro que los **LLMs pueden transformar nuestro enfoque de la enumeración activa y pasiva**. En este caso práctico, utilizamos un modelo de lenguaje para generar una wordlist especializada, basada en la estructura y estilo de nombres del objetivo.

Esto nos permitió sustituir lo que antes era un trabajo manual y tedioso por un proceso **automatizado, rápido y más preciso**. Al personalizar la wordlist con inteligencia contextual, aumentamos significativamente las probabilidades de descubrir subdominios reales, mejorando la cobertura de nuestra fase de reconocimiento.

