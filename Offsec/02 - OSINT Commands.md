---
id: 2025051001
tags:
  - Linux
  - Offsec
  - Cibersecurity
  - español
  - Console
  - Commands
type: permanent
author: juan
---

## 1. Reconocimiento **pasivo** (OSINT)

|Herramienta|Propósito común|Comandos esenciales|Variantes útiles|
|---|---|---|---|
|**WHOIS**|Datos de registro de dominio/IP|`whois megacorpone.com`|`whois -h whois.gandi.net <dominio>`|
|**Google dorks**|Encontrar ficheros sensibles, directorios listados, tecnologías|`site:megacorpone.com filetype:pdf`|`intitle:"index of" -html``inurl:admin|
|**Shodan**|Inventario de servicios expuestos a Internet|`shodan host megacorpone.com`|`shodan search "hostname:megacorpone.com port:22"`|
|**Netcraft (web)**|Stack, sub-hosts, histórico|Usar [https://searchdns.netcraft.com](https://searchdns.netcraft.com)||
|**Repos públicos (GitHub / GitLab)**|Credenciales, código, claves|`gh search code --owner megacorpone password`|`gitleaks detect --source <repo>`|

## 2. Enumeración **DNS**

**2.1 `host` (Linux)**

```bash
host -t mx megacorpone.com            # Registros MX
host -t txt megacorpone.com           # Registros TXT
for s in $(cat subs.txt); do host $s.megacorpone.com; done  # Fuerza bruta
```

**2.2 `dnsrecon`**

```bash
dnsrecon -d megacorpone.com -t std            # Enum estándar
dnsrecon -d megacorpone.com -D subs.txt -t brt # Brute subdominios
```

**2.3 `dnsenum`**

```bash
dnsenum megacorpone.com                       # Todo en uno
dnsenum -f subs.txt -r megacorpone.com        # + dictionary + reverse
```

**2.4 `nslookup` (Windows)**

```bash
dnsenum megacorpone.com                       # Todo en uno
dnsenum -f subs.txt -r megacorpone.com        # + dictionary + reverse
```

---

**3 · Escaneo ligero con **netcat****

|Modo|Sintaxis|Qué revela|
|---|---|---|
|**TCP ping**|`nc -nvv -w1 -z <IP> 3388-3390`|Puertos abiertos/cerrados|
|**UDP ping**|`nc -nv -u -z -w1 <IP> 120-123`|Servicios UDP visibles|
|**Sesión interactiva**|`nc <IP> <puerto>`|Banners / flags|

## 4 · Escaneo **Nmap**

### 4.1 Descubrimiento & barrido inicial

```bash
nmap -sn 192.168.50.0/24                 # Ping sweep
nmap --top-ports 20 <rango> -oG top.txt  # Top-20 puertos + salida greppable
```

### 4.2 Escaneo de puertos

| Objetivo              | Comando                  |
| --------------------- | ------------------------ |
| **TCP básico (1000)** | `nmap <IP>`              |
| **SYN “stealth”**     | `sudo nmap -sS <IP>`     |
| **Connect (-sT)**     | `nmap -sT <IP>`          |
| **UDP**               | `sudo nmap -sU <IP>`     |
| **Mixto**             | `sudo nmap -sS -sU <IP>` |
| **Rango completo**    | `nmap -p 1-65535 <IP>`   |

### 4.3 Fingerprint y servicios

|Tarea|Comando|
|---|---|
|SO fingerprint|`sudo nmap -O <IP> --osscan-guess`|
|Banner/versión|`nmap -sV <IP>`|
|Scan agresivo|`nmap -A <IP>`|

### 4.4 Nmap Scripting Engine (NSE)

```bash
nmap --script http-title -p 80,443 192.168.124.0/24
nmap --script-help http-headers      # Ver doc. del script
```

---

## 5 Medir tráfico con **iptables**

```bash
sudo iptables -I INPUT 1 -s <IP> -j ACCEPT   # Regla temporal
sudo iptables -Z                             # Reset contadores
sudo iptables -vn -L                         # Ver paquetes/bytes
```

---

## 6. **“Living off the Land” en **Windows****

|Herramienta|Ejemplo|Uso|
|---|---|---|
|**Test-NetConnection**|`Test-NetConnection -Port 445 192.168.50.151`|Ping + prueba TCP|
|**One-liner PowerShell**|`1..1024|% { if((New-Object Net.Sockets.TcpClient).Connect('192.168.50.151',$_)){"TCP $_ abierto"} } 2>$null`|
|**xfreerdp** (desde Kali)|`xfreerdp /u:user /p:pass /v:IP`|Acceso RDP|

## 7. Utilidades

- **curl / wget** – pruebas HTTP (`curl -I http://<IP>`).
    
- **grep / cut / seq / awk** – parsear salidas (`grep open result.txt`).

---

## **Enumeración SMB**

| Herramienta / Comando             | Propósito                                                 | Ejemplo                                                       |
| --------------------------------- | --------------------------------------------------------- | ------------------------------------------------------------- |
| `nmap -p 139,445`                 | Escanear puertos SMB y NetBIOS                            | `nmap -v -p 139,445 -oG smb.txt 192.168.50.1-254`             |
| `nbtscan`                         | Enumerar nombres NetBIOS en una red                       | `sudo nbtscan -r 192.168.50.0/24`                             |
| `nmap --script smb-os-discovery`  | Descubrir sistema operativo y detalles de dominio vía SMB | `nmap -v -p 139,445 --script smb-os-discovery 192.168.50.152` |
| `ls /usr/share/nmap/scripts/smb*` | Listar scripts NSE disponibles relacionados con SMB       | `ls -1 /usr/share/nmap/scripts/smb*`                          |
| `net view \\host /all`            | Enumerar recursos compartidos desde cliente Windows       | `net view \\dc01 /all`                                        |
| `enum4linux -a <IP>`              | Enumeración total: usuarios, shares, grupos, SID          | `enum4linux -a 192.168.140.13`                                |
| `cat smb.txt \| grep Up`          | Filtrar hosts activos con SMB                             |                                                               |

---

## Enumeración SMTP

| Herramienta / Comando               | Propósito                                           | Ejemplo                                                  |
| ----------------------------------- | --------------------------------------------------- | -------------------------------------------------------- |
| `nc` (Netcat)                       | Interactuar con el servicio SMTP directamente       | `nc -nv 192.168.50.8 25`                                 |
| `VRFY <usuario>`                    | Verificar existencia de cuentas de correo           | `VRFY root`, `VRFY johndoe`                              |
| Script Python (`smtp.py`)           | Automatizar verificación con VRFY                   | `python3 smtp.py root 192.168.50.8`                      |
| `Test-NetConnection -Port 25 <IP>`  | Verificar conectividad al puerto SMTP desde Windows | `Test-NetConnection -Port 25 192.168.50.8`               |
| `dism /Enable-Feature TelnetClient` | Activar Telnet en Windows                           | `dism /online /Enable-Feature /FeatureName:TelnetClient` |
| `telnet <IP> 25`                    | Usar Telnet para interactuar con SMTP desde Windows | `telnet 192.168.50.8 25`                                 |

---

## **Enumeración SNMP**

|Herramienta / Comando|Propósito|Ejemplo|
|---|---|---|
|`nmap -sU -p 161`|Detectar servicios SNMP habilitados|`sudo nmap -sU --open -p 161 192.168.50.1-254 -oG open-snmp.txt`|
|`onesixtyone`|Fuerza bruta de community strings|`onesixtyone -c community -i ips`|
|`snmpwalk -c public -v1 <IP>`|Recorrer el árbol MIB completo|`snmpwalk -c public -v1 -t 10 192.168.50.151`|
|`snmpwalk <OID>`|Obtener procesos activos|`snmpwalk -c public -v1 192.168.50.151 1.3.6.1.2.1.25.4.2.1.2`|
|`snmpwalk <OID>`|Ver software instalado|`snmpwalk -c public -v1 192.168.50.151 1.3.6.1.2.1.25.6.3.1.2`|
|`snmpwalk <OID>`|Ver puertos TCP en escucha|`snmpwalk -c public -v1 192.168.50.151 1.3.6.1.2.1.6.13.1.3`|
|`snmpwalk <OID>`|Listar usuarios del sistema|`snmpwalk -c public -v1 192.168.50.151 1.3.6.1.4.1.77.1.2.25`|

---
## **Enumeración DNS con LLM + Gobuster**

|Herramienta / Comando|Propósito|Ejemplo|
|---|---|---|
|Prompt para LLM (ChatGPT)|Generar wordlist de 1000 subdominios personalizados|Ver sección: "Listing 67 - LLM's prompt to generate..."|
|`sudo apt install gobuster`|Instalar Gobuster en Kali Linux|`sudo apt install gobuster`|
|`gobuster dns -d <dominio> -w <lista> -t 10`|Enumeración de subdominios con wordlist generada por LLM|`gobuster dns -d megacorpone.com -w wordlist.txt -t 10`|



### FLUJO

1. **OSINT**  
    → Delimita dominios, rangos IP, posibles empleados, leaks, y sistemas expuestos.
    
2. **DNS enum (pasiva y activa)**  
    → Descubre subdominios, registros MX, correos y DNS mal configurados.  
    → Usa herramientas como:
    
    - `nslookup`, `dig`, `amass`, `crt.sh`, `subfinder`, `theHarvester`.
        
3. **LLM-Assisted DNS Wordlist Generation**  
    → Genera wordlists personalizadas con ayuda de LLMs (ej. ChatGPT).  
    → Prompt estructurado para crear una lista de 1000 subdominios basada en la organización.  
    → Usar con `gobuster dns -d dominio -w wordlist.txt -t 10`.
    
4. **Barrido Nmap (-sn)**  
    → Detecta hosts activos.  
    → `nmap -sn 192.168.50.0/24 -oG alive.txt`
    
5. **Escaneo selectivo de servicios (-sS/-p)**  
    → Verifica servicios críticos como HTTP, SMB, SNMP, SMTP.  
    → `nmap -sS -p 21,22,25,53,80,139,445,161,389,3389 <IP>`
    
6. **Fingerprint & NSE**  
    → Descubre versión exacta de servicios y posibles vulnerabilidades con scripts NSE.  
    → Ej: `nmap -sV --script smb-os-discovery -p 139,445 <IP>`
    
7. **Enumeración SMB**
    
    - Descubre nombres NetBIOS: `nbtscan -r 192.168.X.0/24`
        
    - Enumera shares y usuarios: `enum4linux -a <IP>`
        
    - Verifica OS vía NSE: `nmap --script smb-os-discovery`
        
    - Desde Windows: `net view \\host /all`
        
    - Útil para ambientes con Active Directory o servicios Samba expuestos.
        
8. **Enumeración SMTP**
    
    - Conexión directa: `nc -nv <IP> 25`
        
    - VRFY manual: `VRFY root`
        
    - Script Python para automatizar: `python3 smtp.py user <IP>`
        
    - Desde Windows: habilita Telnet y ejecuta `telnet <IP> 25`
        
    - Objetivo: enumerar usuarios válidos o listas de correo.
        
9. **Enumeración SNMP**
    
    - Detectar dispositivos SNMP: `nmap -sU -p 161 <rango>`
        
    - Fuerza bruta de community strings: `onesixtyone -c community -i ips`
        
    - Recorrer árbol MIB completo: `snmpwalk -c public -v1 <IP>`
        
    - Consultas útiles:
        
        - Procesos: `1.3.6.1.2.1.25.4.2.1.2`
            
        - Software instalado: `1.3.6.1.2.1.25.6.3.1.2`
            
        - Puertos en escucha: `1.3.6.1.2.1.6.13.1.3`
            
        - Usuarios: `1.3.6.1.4.1.77.1.2.25`
            
10. **LOLBAS Windows**  
    → Ejecuta binarios legítimos con funciones ofensivas cuando estás limitado a workstations Windows.  
    → Ej: `certutil`, `mshta`, `rundll32`, `regsvr32`, etc.
    
11. **Medición iptables / Red**  
    → Evalúa tráfico generado durante escaneos para mantener bajo perfil.  
    → Usa tcpdump, Wireshark, logs, y revisa reglas en firewalls si tienes acceso.
    
12. **Documenta todo**  
    → IP, puertos abiertos, servicios, banners, scripts NSE relevantes, y hallazgos.  
    → Inicia borrador de reporte técnico desde la fase de escaneo.