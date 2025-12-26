
Dans cet environnement de laboratoire, vous disposerez d'un accès graphique à une machine Kali Linux. Deux machines sont accessibles aux adresses http://target1.ine.local et http://target2.ine.local  

Outils conseillés :  
* nmap  
* Hydra  
* Cadaver  
* Metasploit Framework 
  
---
### Flags
**🏴‍☠️ 1) L'utilisateur « bob » n'a peut-être pas choisi un mot de passe robuste. Essayez des mots de passe courants. (target1.ine.local)**  

**🏴‍☠️ 2) Les fichiers importants se trouvent souvent sur le lecteur C:. Explorez-le minutieusement. (target1.ine.local)**  

**🏴‍☠️ 3) Les partages SMB peuvent contenir des fichiers cachés. Vérifiez les partages disponibles. (target2.ine.local)**  

**🏴‍☠️ 4) Le répertoire Bureau contient peut-être ce que vous cherchez. Parcourez son contenu. (target2.ine.local)**  

---

### Phase de reconnaissance  

<details>

<summary><h3> :arrow_forward: On scanne la cible<h3></summary>  

💡 **On identifie quelques ports intéressants avec les versions et les scripts par défault. C'est une machine Microsoft car serveur IIS.**  

![alt text](<../Images/Capture d'écran 2025-12-26 211028.png>)

:gear: **On procède dans l'ordre, le port 80, on se connecte sur le site et on voit que l'on tombe sur un Username/password**  

![alt text](<../Images/Capture d'écran 2025-12-26 212834.png>)

:bulb: **Recherche des scripts Webdav**
![alt text](<../Images/Capture d'écran 2025-12-26 213056.png>)

:gear: **On lance un script adapté mais comme la page est protégée par une authentification, on ne peut pas savoir si un serveur Webdav est présent car les scripts sont bloqués. Pas grave on passe à l'énumération**  

![alt text](<../Images/Capture d'écran 2025-12-26 213249.png>)

</details>

### Phase d'Exploitation avec Hydra 

#### Attaque avec hydra pour se connecter au site Web
:bulb: On sait que l'identifiant est "bob" et le mot de passe est faible
![alt text](<../Images/Capture d'écran 2025-12-26 215238.png>)

>🟢**User : bob**
>🟢**Password : password_123321**




#### Lancement de `davtest`  
>:bulb: Rappel : davtest permet de s'authentifier sur un service WebDAV et de vérifier si >on peut uploader des fichiers et de quels types, mais aussi les droits (exécutés >ou lecture uniquement).
``

