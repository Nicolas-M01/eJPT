

### Enumérer info PHP
sur la page Web, dans l'URL, on peut rajouter `/phpinfo.php` pour obtenir des infos sur le PHP installé sur le site. Et les "cgi" présentes et vulnérables.
`searchsploit php cgi` permet de rechercher les vuln sur php cgi. Searchploit nous sort une faille exploitable et le module dispo sur metasploit.  
Démarréer Metasploit, puis choisir le module `exploit/multi/http/php_cgi_arg_injection`.  
Renseigner la cible comme d'hab avec RHOSTS, puis lancer.  
On obtient un meterpreter.  







