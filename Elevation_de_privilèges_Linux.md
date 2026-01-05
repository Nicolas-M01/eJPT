
## Cron
Cron est un outil qui permet de plaifier des tâches sous Linux. Il permet de lancer des appli, des scripts et autres commandes de manière répétée ou de façon spécifique.  
Une appli ou script qui est configuré pour s'exécuter de manière répétée est un Cron job.  
Crontab est le fichier de config qui est utilisé par Cron pour stocker et suivre les Cron jobs créés.  
Tout ce qui est lancé avec Cron est en mode "root".  

Exemple

**Vérifier les droits de l'utilisateur courant**
Il n'a pas de privilèges (pas dans le groupe sudoers)  
![alt text](<Images/Capture d'écran 2026-01-04 214345.png>)

**Vérifier les fichiers et tenter de les ouvrir mais accès refusé**  
![alt text](<Images/Capture d'écran 2026-01-04 214550.png>)

**`find / -name message` pour rechercher les fichiers équivalents sur le disque**  
![alt text](<Images/Capture d'écran 2026-01-04 214705.png>)

**Parcourir tous les fichiers sous /usr et afficher chaque ligne qui contient /tmp/message, avec le chemin du fichier et le numéro de ligne**
![alt text](<Images/Capture d'écran 2026-01-04 215106.png>)
* grep : recherche une chaîne de texte  
* "/tmp/message" : texte recherché   
* /usr : répertoire de départ de la recherche  
* -r : recherche récursive dans tous les sous-répertoires de /usr  
* -i : insensible à la casse  
* -n : affiche le numéro de ligne  

**Vérifier les droits du script**
Il est modifiable par le user actuel.  
![alt text](<Images/Capture d'écran 2026-01-04 215745.png>)

**Lorsqu'il n'y a aucun éditeur de texte il faut utiliser ``printf``**  
Lors de leur exécution, ces lignes ajouteront une nouvelle entrée au fichier /etc/sudoers qui permettra à l'utilisateur étudiant d'utiliser sudo sans fournir de mot de passe:  
**`printf '#! /bin/bash\necho "student ALL=NOPASSWD:ALL" >> /etc/sudoers' > /usr/local/share/copy.sh`**  

**Vérification de la liste des `sudoers`**  
![alt text](<Images/Capture d'écran 2026-01-04 220332.png>)

**Nous avons en effet les droits rooot...**  
![alt text](<Images/Capture d'écran 2026-01-04 220606.png>)


## SUID  
SUID : Set Owner User ID  
Cette permission fournit à l'utilisateur le droit d'exécuter un script ou un binaire avec les permissions du propriétaire du fichier. On obtient une permission "root" sur le fichier.  

**On vérife nos droits avec le user actuel et on affiche le contenu du dossier courant**  
Nous somme un user sans privilège. Mais un fichier est de type SUID  
![alt text](<Images/Capture d'écran 2026-01-05 215728.png>)

### * `file my_file` permet de confirmer qu'il est exploitable, de comprendre le format et son linking  


![alt text](<Images/Capture d'écran 2026-01-05 220919.png>)


![alt text](<Images/Capture d'écran 2026-01-05 221259.png>)
