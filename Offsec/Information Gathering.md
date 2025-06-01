
Intro:

The goal of a penetration test (or pentest) is to detect security gaps to improve the defenses of the client organization. Because the network, devices, and software within the company's environment change over time, penetration testing is a cyclic activity. A company's attack surface changes periodically due to newly-discovered software vulnerabilities, configuration mistakes from internal activities, or IT restructuring that might expose new segments for targeting.

Additionally, changes such as onboarding new employees, introducing new products, deploying new machines, expanding infrastructure, or launching websites can further broaden the attack surface, necessitating regular reassessments.


## 6.1. The Penetration Testing Lifecycle

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

## 6.2. Passive Information Gathering

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


## 6.2.1. Whois Enumeration

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


