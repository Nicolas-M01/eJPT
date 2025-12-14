
## Escalade de privilèges

Dans Metasploit :
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





