

Windows OS range les hashes des passwords des utilisateurs dans la database SAM (Security Accounts Manager)  
Un hash est le procédé de convertir une donnée en une autre valeur. Une fonction de hashage ou algorythme est utilisé pour générer une nouvelle valeur. Le résultat est le hash.  
L'authentification et la vérification des credentials est facilité par Local Security Authority (LSA).  
Windows jusqu'aux version Win Serv 2003 utilisait 2 différents types de hashes :  
* LM  
* NTLM  
  
LM a été arrêté utilise les hashes NTLM depuis VISTA.  

## SAM
* SAM (Security Account Manager) est une base de données Windows qui gère les comptes utilisateurs et leurs mots de passe, stockés sous forme hachée.  
* Le fichier SAM est verrouillé pendant que Windows fonctionne, donc il ne peut pas être copié directement.  
* Le noyau Windows NT protège le SAM ; pour cette raison, les attaques ciblent souvent la mémoire, notamment le processus LSASS, afin d’extraire les hash.  
* Dans les versions modernes de Windows, le SAM est chiffré à l’aide d’une clé système (syskey).  
* Des privilèges administrateur/élevés sont nécessaires pour accéder au processus LSASS.  
👉 En bref : Windows protège fortement les mots de passe via le SAM, le chiffrement et le verrouillage mémoire, et leur accès nécessite des droits élevés.  

## LM (LanMan)  
* LM (LAN Manager) est un ancien algorithme de hachage utilisé par Windows avant NT 4.0.  
* Le mot de passe est :  
  * découpé en deux blocs de 7 caractères,  
  * converti en majuscules,  
  * puis chaque bloc est haché séparément avec DES.  
* LM est très faible en sécurité : absence de sel (salt), découpage prévisible et DES rendent les mots de passe faciles à casser par force brute ou tables arc-en-ciel.  
👉 En bref : LM est obsolète et dangereux, raison pour laquelle il est désactivé sur les systèmes Windows modernes.  


## NTLM (NTHash) 
* NTLM (NTHash) est un ensemble de protocoles d’authentification utilisés par Windows pour vérifier l’identité des utilisateurs entre machines.  
* À partir de Windows Vista, le hachage LM est désactivé au profit de NTLM.  
* Le mot de passe utilisateur est haché avec l’algorithme MD4, puis le mot de passe en clair est supprimé.  
* NTLM améliore LM car il :  
  * ne découpe pas le mot de passe en blocs,  
  * est sensible à la casse,  
  * supporte les symboles et caractères Unicode.  
👉 En bref : NTLM est plus sécurisé que LM, mais reste aujourd’hui moins robuste que les mécanismes modernes (ex. Kerberos).  

--- 

## Mimikatz  
Outil post-exploitation. Il permet l'extraction de passwords en texte clair, de hases, et de tickets Kerberos. SAM est la DB où sont stockés les hashes de passwords.  
Mimikatz peut extraire les hashes de lsass.exe où se trouvent les hashes.  

## Pass The Hash Attack avec Mimikatz
### 👉 Après scan et identification vul BadBlue 2.7
![alt text](<Images/Capture d'écran 2025-12-22 194313.png>)
`set target` pour changer de version  de badlue. (EE pour enterprise edition par exemple...)  

### 👉 Lancer le module, vérifier, migrer vers lsass...
![alt text](<Images/Capture d'écran 2025-12-22 194745.png>)

### 👉 Kiwi module (Metasploit)
**Dump des hashes NTLM**
![alt text](<Images/Capture d'écran 2025-12-22 195055.png>)
**Dump des secrets** Dans certains cas on peut obtenir un mot de passe en clair texte.  
`lsa_dump_sam` pour récupérer les hashs des copmtes  
`creds_all` : pour l'utilisateur actuel  
![alt text](<Images/Capture d'écran 2025-12-22 195107.png>)

### 👉 Upload de `mimikatz` sur cible 
![alt text](<Images/Capture d'écran 2025-12-22 195539.png>)

### 👉 Lancement de `mimikatz et dumpe des hashes`
![alt text](<Images/Capture d'écran 2025-12-22 195904.png>)

### 👉 dump des secrets puis récupération des passwords en clair si système mal configuré (pas dans notre cas présent)
![alt text](<Images/Capture d'écran 2025-12-22 200336.png>)
![alt text](<Images/Capture d'écran 2025-12-22 200350.png>)


---

## Pass The Hash Attack avec Kiwi+PSexec et alternative Kiwi+crackmapexec (intégré à Metasploit)
Contexte : Nous avons une cible Windows vulnérable à BadBlue 2.7.
### 👉 Scan de la cible et on voit BadBlue 2.7  
![alt text](<Images/Capture d'écran 2025-12-21 174841.png>)

### 👉 On lance le module Metasploit qui exploit cette vuln BadBlue 2.7
On paramètre la cible et on obtient le meterpreter  
![alt text](<Images/Capture d'écran 2025-12-21 175340.png>)

### 👉 Migration vers lsass puis lancement de kiwi
![alt text](<Images/Capture d'écran 2025-12-21 175606.png>)

### 👉 Récupération des Hashes avec `lsa_dump_sam`
![alt text](<Images/Capture d'écran 2025-12-21 180105.png>)

### 👉 Récupération des Hashes LM et Hashes NTLM de tous les users avec `hashdump`  
![alt text](<Images/Capture d'écran 2025-12-21 180308.png>)

### 👉 Lancement de `psexec`  
Toujours dans Metasploit `ctrl+z` pour mettre la sessionen background, puis `search psexec`, puis choisir `exploit/windows/smb/psexec`  

### Config de `psexec`
Suivre la procédure comme ci dessous et on devrait obtenir un meterpreter (Bien copier hash LM+NTLM)  
![alt text](<Images/Capture d'écran 2025-12-22 190740.png>)


### ❗Alternative sans Metasploit :S'identifier sur la cible avec `crackmapexec`
![alt text](<Images/Capture d'écran 2025-12-22 192217.png>)
![alt text](<Images/Capture d'écran 2025-12-22 192235.png>)
