

Sur machine Windows, ouvrir Powershell :
* `whoami /getpriv` : nous donne les privilèges de l'utilisateur actuel.  
  
![alt text](<Images/Capture d'écran 2025-12-21 145240.png>)


#### 👉 Générer un payload depuis la machine attaquante.  
![alt text](<Images/Capture d'écran 2025-12-21 150040.png>)  

`-p windows/x64/meterpreter/reverse_tcp`  

Définit le payload à utiliser.
Décomposition :
windows → système cible
x64 → architecture 64 bits
meterpreter → payload avancé Metasploit
reverse_tcp → la victime initie la connexion vers l’attaquant
📌 Pourquoi reverse shell ?  
Contourne plus facilement les firewalls Plus réaliste en environnement réel.  

#### 👉 Lance un serveur web HTTP très simple sur la machine, dans le dossier courant, pour partager des fichiers:   

![alt text](<Images/Capture d'écran 2025-12-21 151235.png>)

(Sur python3, l'équivalent est : `python3 -m http.server 80`)  
Cette commande nous permet d'avoir accès à la machine victime, pour ensuite transférer un fichier que la victime pourra télécharger.  

#### 👉 Sur la VM Windows on télécharge le payload:  
![alt text](<Images/Capture d'écran 2025-12-21 152324.png>)  


#### 👉 On peut voir sur la VM d'attaque que le payload a été téléchargé:  
![alt text](<Images/Capture d'écran 2025-12-21 152506.png>)  

*  On quitte ensuite le serveur web en écoute sur la VM d'attaque : `ctrl+C`  
  

#### 👉 Démarrer Metasploit  
![alt text](<Images/Capture d'écran 2025-12-21 152753.png>)

