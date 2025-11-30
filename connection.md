# Mysql  
`mysql -u <user> -p<password> -h <IP_TARGET>`  


# FTP  
`ftp <IP-TARGET>`

# HYDRA  
`hydra [options] -L (ou -l) liste_users -P liste_mdp ftp://IP_ou_Domaine`  

-l = un seul nom d’utilisateur
-L = un fichier contenant plusieurs noms d’utilisateur
-p = un seul mot de passe
-P = un fichier contenant plusieurs mots de passe

# SMB
Se connecter à un SMB client qui autorise la connection anonyme sans mot de passe.    
`smbclient //IP_TARGET/MY_SHARE`
`-N` si pas de mot de passe  

# enum4linux
Obtenir des infos sur les partages SMB.  
`enum4linux IP_ADDRESS`
`-s` : Afficher une wordlist de partages.  
