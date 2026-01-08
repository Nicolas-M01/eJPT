
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

<summary><h3> :arrow_forward: On scanne la cible<h3></summary>  

