
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

[_Whois_](https://www.domaintools.com/support/what-is-whois-information-and-why-is-it-valuable/) is a TCP service, tool, and type of database that can provide information about a domain name, such as the [_name server_](https://www.forbes.com/advisor/business/software/what-is-a-name-server/) and [_registrar_](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name-registrar/). This information is often public, since registrars charge a fee for private registration.

We can gather basic information about a domain name by executing a standard forward search and passing the domain name, **megacorpone.com**, into `whois`, providing the IP address of our Ubuntu WHOIS server as an argument of the host (`-h`) parameter

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


