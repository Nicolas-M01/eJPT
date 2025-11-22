# Nmap

Bienvenue  

Il y 65535 ports sur un ordinateurs. Les 1024 premiers sont les ports bien connus.  


## :clipboard: Some common switches :  

``-sS`` : SYN Scan TCP  
``-sT`` : Connect Scan TCP  
``-sU`` : Scan UDP  
`-sn` : ne scan pas, envoie des paquets ICMP pour détecter les hôtes présents.
``-O`` : Get operating System  
``-sV`` : Version du service.  
`--version-intensity <0–9>` : Intensité des sondes de `sV`, valeur par défaut "7". Scan plus long et plus bruyant mais plus précis dans la version.  
`-Pn` : Treat all hosts as online -- skip host discovery  
`-F` : Mode "fast", les 100 ports les plus courants.  
`-v` : verbose,  `-vv` : verbose+ ...  
``-A`` : Agressive mode Enable OS detection (-O), version detection (-sV), script scanning (-sC), and traceroute (--traceroute)  
`-T5` : Increase speed (from 0 to 5), be careful : higher speeds are noisier, and can incur errors. T3 est la vitesse par défaut.    
`-p 80` : scan only port 80.  
`-p 1000-1500` : Scan ports 1000 to 15000  
`-p-` : Scan ***All*** ports  
`-sC` : Exécute la collection de scripts marqués "default" dans l’écosystème NSE (Nmap Scripting Engine). Ces scripts effectuent des tâches d’énumération et de vérification courantes. Les scripts "default" fournissent des infos utiles sans être trop intrusif.  
`--script` : Activate a script from nmap library  
`--script=vuln` : Activate all the scripts in the "vuln" category.  
`ls -la /usr/share/nmaps/scripts/ | grep -e "ftp"` : Liste les scripts de scan contenant le nom "ftp". Ce qui permet ensuite de lancer nmap avec un script précis.  
To run a specific script, we would use ``--script=<script-name>``  

**Firewall/IDS évasion**  
`-f` : Fragmentation des paquets envoyés pendant le scan (généralement 8 octets chacun) (--mtu pour ajuster la taille max du fragment). Cela permet d'éviter les firewalls et IDS/IPS.  
`--data-length <nombre>` : Ajouter un certain nombre d'octets aléatoires aux paquets envoyés par nmap. Evite les détections IDS/IPS, moins reconnaissable. Utile en obfuscation. Exemple : `nmap --data-length 50 192.168.1.10` (Ajoute 50 octets aléatoires à chaque paquet envoyé.)  
`-D <leurre1,leurre2,...,ME <IP cible>` : Envoie de leurres (Decoys). Envoie des paquets avec plusieurs adresses IP sources fausses, pour masquer l’origine réelle du scan, `ME` suivi de la vraie IP cible pour pouvoir recevoir les paquets.  
`-g <port>` : modifier le port d'origine. Afin de tenter de contourner des règles simples de firewall/ACL  


`--host-timeout <time>` : Abandonne la cible après le temps défini.  

**Output Formats**
`-oA` : Save in the 3 majors formats (normal, XML, Grepable)  
`-oG` : Save results in "grepable" format  
`-oN` : Save results in normal format  
`-oX` : Save results in XML format  



## 3 basics scans  

These are used in most situations  
####  1) TCP Connect Scans (``-sT``)  

**Sans privilège (non Root et non Admin), nmap lancera un TCP connect scan (-sT). Le connect fait la connexion TCP complète (3‑way handshake) via l'API système (plus bruyant, plus détectable).**  

If Nmap sends a TCP request with the SYN flag set to a _closed_ port, the target server will respond with a TCP packet with the RST (Reset) flag set and Nmap can establish that the port is closed  
If, however, the request is sent to an _open_ port, the target will respond with a TCP packet with the SYN/ACK flags set. Nmap then marks this port as being open (and completes the handshake by sending back a TCP packet with ACK set).  
If the port is open but hidden behind a firewall, the firewall will _drop_ incomings packets. Nmap sends a TCP SYN request, and receives nothing back. This indicates that the port is being protected by a firewall and thus the port is considered to be filtered.  
If a port is closed, the flag RST is sent back from the target.  

#### 2) SYN "Half-open" Scans (``-sS``)  

**Par défaut, nmap effectue un SYN scan (-sS) si vous avez les privilèges nécessaires (sous Unix : root; sous Windows : exécuté en administrateur et avec un driver pcap/npcap).  
Le SYN scan est un scan « half‑open » : envoie un SYN, attend SYN+ACK puis envoie RST (pas de connexion TCP complète).**  


As with TCP scans, SYN scans (-sS) are used to scan the TCP port-range of a target or targets; however, the two scan types work slightly differently. SYN scans are sometimes referred to as "Half-open" scans, or "Stealth" scans.  
Where TCP scans perform a full three-way handshake with the target, SYN scans sends back a RST TCP packet after receiving a SYN/ACK from the server (this prevents the server from repeatedly trying to make the request)  

**Advantages :**  
* It can be used to bypass older Intrusion Detection systems as they are looking out for a full three way handshake. This is often no longer the case with modern IDS solutions; it is for this reason that SYN scans are still frequently referred to as "stealth" scans.  
* SYN scans are often not logged by applications listening on open ports, as standard practice is to log a connection once it's been fully established.  
* Without having to bother about completing (and disconnecting from) a three-way handshake for every port, SYN scans are significantly faster than a standard TCP Connect scan  

**Drawbacks :**  
* They require sudo permissions in order to work correctly in Linux.  
* Unstable services are sometimes brought down by SYN scans  

SYN scans are the default scans used by Nmap if run with sudo permissions  

If a port is _closed_ then the server responds with a RST TCP packet.  
If the port is filtered by a firewall then the TCP SYN packet is either dropped, or spoofed with a TCP reset  



#### 3) UDP Scans (``-sU``)  
Unlike TCP, UDP connections are stateless. This means that, rather than initiating a connection with a back-and-forth "handshake", UDP connections rely on sending packets to a target port and essentially hoping that they make it.  
When a packet is sent to an open UDP port, there should be no response. When this happens, Nmap refers to the port as being open|filtered. In other words, it suspects that the port is open, but it could be firewalled. If it gets a UDP response (which is very unusual), then the port is marked as open. More commonly there is no response, in which case the request is sent a second time as a double-check. If there is still no response then the port is marked open|filtered and Nmap moves on.  

When a packet is sent to a closed UDP port, the target should respond with an ICMP (ping) packet containing a message that the port is unreachable. This clearly identifies closed ports, which Nmap marks as such and moves on.  

 UDP scans tend to be incredibly slow in comparison to the various TCP scans, so ``nmap -sU --top-ports 20 <target>`` allows nmap to scan the most commonly used 20 UDP ports, that reduces the time.  

When UDP port is closed , we receive an ICMP packet "port unreachable".

## Additional port scan types  

### 1) TCP Null Scans (``-sN``)  
As the name suggests, NULL scans (-sN) are when the TCP request is sent with no flags set at all. As per the RFC, the target host should respond with a RST if the port is closed.  

### 2) TCP FIN Scans (``-sF``)  
FIN scans (-sF) work in an almost identical fashion; however, instead of sending a completely empty packet, a request is sent with the FIN flag (usually used to gracefully close an active connection). Once again, Nmap expects a RST if the port is closed.  

### 3) TCP Xmas Scans (``-sX``)  
As with the other two scans in this class, Xmas scans (-sX) send a malformed TCP packet and expects a RST response for closed ports. It's referred to as an xmas scan as the flags that it sets (PSH, URG and FIN) give it the appearance of a blinking christmas tree.  













