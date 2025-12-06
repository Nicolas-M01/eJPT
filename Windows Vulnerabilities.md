
## Types de vulnérabilités Windows

* Divulgaton d'informations  
* Buffer offerflows  
* Execution de code à distance  
* Escalade de privilèges  
* DoS  

---

## Services Windows fréquemment exploités et outils de pentest 

* Microsoft IIS  
* WebDAV (Web Distributed Atuhoring & Versionning)  
  * davtest : utilisé pour scanner, authentifier et exploiter un server WebDAV.  
  * cadaver : supporte l'envoi, le téléchargement, l'affichage, l'édition, copie/déplacement, création de collections, suppression, manipulation et verrouillage des ressources sur des serveurs WebDAV.  
  (`--script http-enum` avec nmap pour cibler cette vulnérabilité)  
* SMB/CIFS  
* RDP  
* WinRM  


### Système/Hôte attaque
 un scan classique nmap avec option `script=http-enum` permet de voir si un service "WebDAV" est actif.  

 #### davtest
 davtest permet de s'authentifier sur un service WebDAV et de vérifier si on peut uploader des fichiers et de quels types, mais aussi les droits (exécutés ou lecture uniquement).  

Voici le résultat de ce que nous pouvons uploader et exécuter avec la commande de dessus :  

![alt text](<Images/Capture d'écran 2025-12-06 124009.png>)

### cadaver  
`cadaver  http://demo.ine.local/webdav` permet de se connecter en tant que client au serveurs WebDAV (on doit s'authentifier après cette commande).  

Une fois connecté, tu peux utiliser des commandes simples, comme :  
``ls`` → voir les fichiers  
``cd`` → naviguer dans les dossiers  
``put`` fichier.ext → envoyer un fichier (si autorisé)  
``get fichier.ext`` → récupérer un fichier  
``delete fichier.ext`` → supprimer un fichier (si autorisé)  
``mkdir nom`` → créer un dossier (si autorisé)  
➡️ Cadaver sert à manipuler le contenu WebDAV, rien de plus.  

`put /usr/share/webshells/asp/webshell.asp` permet l'upload d'un webshell.  

Une fois la backdoor (webshell installée), on peut se reconnecter depuis le site web, s'authentifier sur le serveur WebDAV puis nous lançons le webshell.  

il suffit de taper `dir c:\` pour lister la racine, puis `type flag.txt` pour lire le flag.  

