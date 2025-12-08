
# SMB

---

## PSEXEC avec Metasploit

* Scan classique avec `-script=smb-protocols` permet de voir les différents protocoles supportés par le serveur.  
* Lancer Metasploit  
* ``auxiliary/scanner/smb/smb_login`` et renseigner les options, pour brute forcer les users/passwords.  
* Une fois user/password récupérés, nous pouvons exécuter `exploit/windows/smb/psexec` et renseigner les bonnes infos (cible, user, password.) puis on lance l'exploit  
* On obtient ensuite un meterpreter. ( `shell`...)  
  

## SMBCLIENT 
Se connecter à un SMB client qui autorise la connection anonyme sans mot de passe.    
`smbclient //IP_TARGET/MY_SHARE`
`-N` si pas de mot de passe  