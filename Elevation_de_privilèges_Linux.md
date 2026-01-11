
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
le répertoire utilisateur contient deux fichiers supplémentaires : « greetings » et « welcome ». Nous n'avons pas la permission d'exécuter le fichier binaire « greetings », mais nous pouvons exécuter le fichier binaire « welcome ».
![alt text](<Images/Capture d'écran 2026-01-05 215728.png>)

### * `file my_file` nous permet de confirmer qu'il s'agit de deux fichiers binaires. Nous pouvons donc utiliser ces fichiers pour élever nos privilèges.  

Vérifions ce qui est utilisé en arrière-plan lors de l'exécution du binaire « welcome ». Pour ce faire, nous pouvons taper strings welcome.
![alt text](<Images/Capture d'écran 2026-01-05 220919.png>)
Cette strings commande permet d'extraire le texte lisible par un humain intégré au fichier binaire. Cela peut nous aider à identifier des informations importantes telles que les noms de fonctions, les chaînes de caractères codées en dur ou toute autre donnée utile susceptible de faciliter une élévation de privilèges ou de révéler des détails sur le fonctionnement du binaire.  



Après avoir examiné le résultat, nous avons découvert que le binaire « welcome » utilise le binaire « greetings », comme le montre l’image ci-dessus.

Nous pouvons supprimer le fichier « greetings » de ce dossier et créer un nouveau fichier « greetings » contenant notre charge utile personnalisée.

Notre charge utile est : cp /bin/bash greetings, qui copiera le shell Bash dans le binaire « greetings », nous permettant de l’exécuter avec des privilèges élevés.
![alt text](<Images/Capture d'écran 2026-01-05 221259.png>)

>**Après avoir effectué ces modifications, exécutez simplement à nouveau le fichier binaire « welcome » en tapant :./welcome**  
