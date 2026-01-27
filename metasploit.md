
# Metasploit

## 🧰 Présentation rapide

Metasploit Framework est un outil de pentest (test d’intrusion) open source, utilisé pour :
* Scanner des failles de sécurité,
* Exploiter des vulnérabilités connues,
* Lancer des payloads,
* Créer et tester des exploits personnalisés.
Il est inclus par défaut dans Kali Linux.

### Architecture 
**Exploit** : Module pour prendre avantage d'une vulnérabilité (généralement fonctionne avec un payload)  
**Payload** : Code fourni par MSF et exécuté à distance sur la cible après une exploitation réussie.  
**Encoder** : Encode les payloads pour éviter les détections des AV.  
**NOPS** : Utiliser pour s'assurer que la taille des payloads est correct et assurer la stabilité d'un payload lorsqu'il est exécuté.  
**Auxiliary** : Un module utilisé pour exécuter des fonctions complémentaires comme le scan de ports et l'énumération.  



---

## ⚙️ Démarrage  
📌 **Vérifier que Metasploit est bien installé :**
```bash
msfconsole --version
```
S'il n'est pas installé, l'installer depuis les dépôts officiels :  
```bash
sudo apt update
sudo apt install metasploit-framework -y
```

📌 **Démarrage de "postgresql"**  
`systemctl enable postegresql` ou `service postgresql start`  

  
📌 **Lancer metasploit**  
``msfconsole``  

📌 **Vérifier le statut de la base**  
```bash
db_status
``` 
doit renvoyer `Connected to msf. Connection type: postgresql` pour vérifier que la base de données est bien connectée.  

---

## 🧭 Commandes de base à connaître
| Commande                  | Description                                     |
| ------------------------- | ----------------------------------------------- |
| ``help``                  | Affiche l’aide                                  |
| ``search nom_du_module``  | Cherche un exploit, un payload ou un auxiliaire |
| ``use chemin/du/module``  | Charge un module                                |
| ``show options``          | Affiche les paramètres nécessaires              |
| ``set PARAM valeur``      | Définit un paramètre                            |
| ``run ou exploit``        | Lance le module                                 |
| ``workspace -a <MyWork>`` | Permet de créer un nouvel espace de travail     |


``db_import /root/myXMLdoc``  

---

**Créer un espace de travail**  
```bash
workspace -a "My_Workspace"  
``` 

**Vérifier sur quel espace de travail nous sommes**  
```bash
workspace  
``` 

**Recherche de modules auxiliaires**  
Exemple :
```bash
search portscan 
``` 
➡️ Va rechercher tous les chemins contenant le nom "portscan"

Pour une recherche affinée parmi une grande liste :
```bash
search type:auxiliary name:smb
```
➡️ Va filtrer en recherchant dans les dossiers "auxiliary", et le nom "smb"


**Choisir son module**
```bash
use auxiliary/path/to/research 
```
ou
```bash
use 4
```
➡️ Ici "4" est le chiffre raccourci du nom du module (auxiliaire)  

![alt text](Images/image.png)

**Afficher options du module choisi et les modifier**  
```bash
show options
```
Il suffit de choisir l'option qui nous intéresse pour lui attribuer une nouvelle valeur, exemple :
```bash
set RHOSTS 192.86.140.3
```
➡️ va cibler la machine à l'adresse IP indiquée.

**Lancer le module (auxiliaire)**
```bash
run
```
ou
```bash
exploit
```

**`analyze`** :  
* Identifier le type de session (Meterpreter, shell, OS, architecture, privilèges).
* Détecter des possibilités d’escalade de privilèges (selon l’OS et la config).  
* Suggérer des modules Metasploit pertinents à utiliser ensuite (post-exploitation).  
* Résumer l’état de la machine compromise sans lancer chaque commande à la main  

**`vulns`** :  
Sert à lister les vulnérabilités connues pour une machine cible à partir des informations déjà collectées  


**Meterpreter**
Meterpreter (interpréteur) se lance sur la cible une fois que l'on est connecté dessus et donc que l'execution du module a réussi.


## Importer scan Nmap dans MSF

Après enregistrement de la sortie de la commande Nmap en format xml (-oX), nous allons importer le scan dans la console MSF.  
Une fois Metasploit démarré, `db_import`, `hosts` permet de voir la machine ciblée et importée. `services` liste tous les ports ouverts.  
* `db_nmap` + options : permet de lancer un nmap intégré directement à msf au workspace actuel. `vulns permet de voir les vulnérabilités`.  
* Module Scan TCP : `auxiliary/scanner/portscan/tcp`  
* Module Scan UDP : `auxiliary/scanner/discovery/udp_sweep`  
  

## searchploit
`searchsploit "Microsoft Windows SMB"` nous fournit tous les exploits en lien avec "Microsoft Windows SMB".  
`searchsploit "Microsoft Windows SMB" | grep -e "Metasploit"` permet de filtrer les exploits utilisables dans Metapsloit.  

## Plugin : Metasploit-Autopawn  
Télécharger le plugin en suivant les instructions sur github.  
`load db_autopwn` charge le plugin installé.  
`db_autopwn` affiches les options.
`db_autopwn -p -t -PI 445` exemple d'utilisation.  

## Scripts Metasploit
La liste de scripts Mtesploit se trouve dans :
`ls -la /usr/share/metasploit-framework/scripts/resource/`  

Lancer msfconsole avec un script spécifique :  
`msfconsole -r handler.rc`  
`resource My_path/My_script.rc`  

### créer un script metasploit
* créer et ouvrir un fichier avec extension `.rc`  
* `use auxiliary/scanner/portscan/tcp`  
* `set RHOSTS 10.10.10.7`  
* `run`  
Ce script va enchainer les commandes une fois lancé.  

Il est possible de générer un script à partir des commandes lancées précédemment:  
`makerc ~/Desktop/test.rc` : il va intégrer les commandes dans le bon ordre.  




