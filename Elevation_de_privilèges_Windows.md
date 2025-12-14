
## Escalade de privilèges

### Metasploit :
`sessions` : Sert à lister et gérer les sessions actives ouvertes via Metasploit  
`getuid` : Affiche l’identité de l’utilisateur sous lequel la session tourne sur la machine distante  
`getprivs` : Liste les privilèges de sécurité accordés au processus courant.  
`post/multi/recon/local_exploit_suggester` : Permet de lister des modules qui permettent d'exploiter le kernel de la machine Windows.  
On peut ensuite choisir le module qui nous intéresse.  
* On peut rechercher l'exploit sur internet, puis copier-coller le chemin du module de l'exploit pour accéder au module, puis renseigner les options, puis on peut obtenir un meterpreter.  
* Le github "AonCyberLabs/Windows-Exploit-Suggester", cloner le projet contenant l'exploit, lire le readme.md (il faut récupérer `sysinfo` depuis le metertpreter de metasploit et le copier-coller).  
* Il faut ensuite aller dans le dossier du projet (Windows-exploit-suggester) et exécuter le script python en le mettant à jour : `./windows-exploit-suggester --update`. Il va mettre à jour la database (visible en direct à l'exécution du script.)  
* Puis `./windows-exploit-suggester --database my_database.xls --systeminfo pathTotextfile`  
Le fichier texte à renseigner est le copier-coller fait depuis le meterpreter.  
* Il va afficher la liste des vulns par rapport au fichier texte de sysinfo.  
On peut récupérer la vuln affichée aller voir le github de cette vuln (ex: MS16-135).  
* Il faut surtout aller voir la vuln (ici MS16-135) dans le dépôt de "AonCyberLabs/Windows-Exploit-Suggester"  
* Il faut ensuite télécharger le ".exe"  
* Utiliser le terminal avec le meterpreter en cours, se déplacer dans "C:\Temp", puis uploader l'exploit de la vuln (MS16-135) : `upload ~/Downloads/41015.exe`  
* `shell` pour lancer un shell, vérifier que le fichier est uploadé. Exécuter le shell, il nous indique qu'il doit être lancé en fonction de l'OS, donc relancer avec la bonne option  
* Si tout est ok, nous avons un nouveau shell avec des droits élevé grâce à l'exploitation du kernel.  


## UAC (User Account Control)
Implémenté depuis Vista, cette fenêtre permet de s'assurer que les changements impactants l'OS ont l'accord de l'admin ou que le user est dans le groupe admin.    

Il est possible de bypasser UAC par différents outils. Tout dépend de la version d'OS de la cible.  

### Metasploit
* `users` Pour connaître les users de la machine  
* `net localgroup administrators` pour connaître les membres du groupe administrators.  
**Démarrer Metasploit** :
* `search rejetto`, puis utiliser le module (après paramétrage des options) et on obtient facilement un meterpreter (`sysinfo` pour vérifier)  
* Migrer d'un meterpreter x86 vers x64 : `pgrep explorer`, une valeur est rencoyée, puis `migrate <Number>`, nous obtenons un meterpreter x64.  
* `getprivs` permet de voir les privilèges du user actuel.  
* `net localgroup administrators` : Nous permet de voir les comptes membres du groupes des admins locaux.  





### UACMe (UACme Github)
Contient une liste de méthodes utilisées pour bypasser UAC sur les Windows de 7 à 10.  




