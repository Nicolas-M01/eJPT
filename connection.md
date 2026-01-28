# Mysql  
`mysql -u <user> -p<password> -h <IP_TARGET>`  


# FTP  
`ftp <IP-TARGET>`

# HYDRA  
`hydra [options] -L (ou -l) liste_users -P liste_mdp ftp://IP_ou_Domaine` il est nécessaire de rajouter `-t 4` pour ralentir la vitesse en ftp pour ne pas être banni.  
* `hydra -L users.txt -P passwords.txt smb://target1.ine.local`
* `hydra -L users.txt -P passwords.txt rdp://target1.ine.local`
* `hydra -L users.txt -P passwords.txt target1.ine.local http-post-form \`  
* `hydra -L users.txt -P passwords.txt target1.ine.local http-get /`
* `hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/common_passwords.txt -t 4 demo.ine.local ssh`  

-l = un seul nom d’utilisateur
-L = un fichier contenant plusieurs noms d’utilisateur
-p = un seul mot de passe
-P = un fichier contenant plusieurs mots de passe

# SMB
Se connecter à un SMB client qui autorise la connection anonyme sans mot de passe.    
`smbclient //IP_TARGET/MY_SHARE`
`-N` si pas de mot de passe  

`smbclient //SERVEUR/PARTAGE -U utilisateur` il nous demandera le mot de passe.



# enum4linux
Obtenir des infos sur les partages SMB.  
`enum4linux IP_ADDRESS`
`-s` : Afficher une wordlist de partages.  


# Gobuster
`gobuster dir -u http://target.ine.local -w /usr/share/wordlists/dirb/common.txt`  


# crackmapexec
* `crackmapexec smb IP -u utilisateur -p motdepasse` Connexion  
* `crackmapexec smb IP -u utilisateur -p motdepasse --shares` : lister les partages  
* `crackmapexec smb IP -u utilisateur -p motdepasse -x "whoami"` Commandes distantes si admin.  
* `crackmapexec smb 192.168.1.10 -u admin -H aad3b435b51404eeaad3b435b51404ee:5f4dcc3b5aa765d61d8327deb882cf99` Pass-the-hash (NTLM)  
* 