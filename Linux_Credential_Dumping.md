
Toutes les infos des comptes Linux se trouve dans `/etc/passwd`.  
Il est accessible par "root"  

| Valeur | Algorythme de Hash |
| ------ | ------------------ |
| $1     | MD5                |
| $2     | Blowfish           |
| $5     | SHA-256            |
| $6     | SHA-512            |

## Mise en pratique

### Scan et version du service
![alt text](<Images/Capture d'écran 2026-01-08 184331.png>)  

### Recherche de vulnérabilité de la version de FTP avec ``searchsploit``  
![alt text](<Images/Capture d'écran 2026-01-08 185153.png>)

### Recherche de vulnérabilité de la version de FTP avec ``Metasploit``  


### Paramétrage du payload, LHOST etc... et lancement  
![alt text](<Images/Capture d'écran 2026-01-08 185842.png>)  

**``/bin/bash -i``** permet de récupérer un shell bash.  
**`sessions`** permet de voir les sessions actives.  
**`session -u 1`** permet d'upgrader la session vers un meterpreter.  

### On affiche le contenu de `/etc/shadow`  
![alt text](<Images/Capture d'écran 2026-01-08 190535.png>)  
Le hash est chiffré en SHA-512  



### Postexploitation Utilisation de `auxiliary/analyze/crack_linux` pour cracker le password  
**On rentre le format d'algorythm trouvé plus tôt dans /etc/shadow  
![alt text](<Images/Capture d'écran 2026-01-08 191639.png>)

