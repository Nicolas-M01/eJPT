
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

--- 

<details>
<summary><h3>:arrow_forward:Flag 1 L'utilisateur « bob » n'a peut-être pas choisi un mot de passe robuste. Essayez des mots de passe courants. (target1.ine.local) <h3></summary>

#### Attaque avec hydra pour se connecter au site Web
:bulb: On sait que l'identifiant est "bob" et le mot de passe est faible
![alt text](<../Images/Capture d'écran 2025-12-26 215238.png>)

>🟢**User : bob**  
>🟢**Password : password_123321**  

#### Enumération du site avec `dirb`
On voit qu'il contient bien une page "WebDAV"
![alt text](<../Images/Capture d'écran 2025-12-27 134026.png>)

#### Lancement de `davtest`  
>:bulb: Rappel : davtest permet de s'authentifier sur un service WebDAV et de vérifier si on peut uploader des fichiers et de quels types, mais aussi les droits (exécutés >ou lecture uniquement).
`davtest -auth bob:password_123321 -url http://target1.ine.local/webdav` permet de se connecter :  

![alt text](<../Images/Capture d'écran 2025-12-26 220808.png>)

>:bulb: **Les fichiers .asp peuvent s'exécuter, nous allons pouvoir lancer cadaver >pour uploader un webshell.**  

#### Lancement de `cadaver`  
![alt text](<../Images/Capture d'écran 2025-12-26 221311.png>)

**On est loggé et un webshell est uploadé, on peut se connecter depuis l'interface web**
![alt text](<../Images/Capture d'écran 2025-12-26 221416.png>)
![alt text](<../Images/Capture d'écran 2025-12-26 221727.png>)

:bulb: **Alternative pour afficher le flag1.txt**  
![alt text](<../Images/Capture d'écran 2025-12-27 134334.png>)

**FLAG 1 trouvé !!!**  
</details>


---

<details>

<summary><h3> :arrow_forward: Flag2 Les fichiers importants se trouvent souvent sur le lecteur C:. Explorez-le minutieusement. (target1.ine.local)</summary><h3>

### Lancement du Webshell

Images/Capture d'écran 2025-12-26 222607.png

:gear: **Il nous reste plus qu'à aller à la racine C:\ pur récupérer le flag2.**  

`dir C:\` nous permet de voir l'emplacement du flag, donc `type C:\flag2.txt`
![alt text](<../Images/Capture d'écran 2025-12-26 222947.png>)

**FLAG 2 trouvé !!!**
</details>

---

<details>

<summary><h3> :arrow_forward: Flag3 Les partages SMB peuvent contenir des fichiers cachés. Vérifiez les partages disponibles. (target2.ine.local)</summary><h3>

#### Enumération de "target2.ine.local"

![alt text](<../Images/Capture d'écran 2025-12-28 160838.png>)

:gear: Nous allons tenter de trouver les credentials SMB par Metasploit, on choisit le module
![alt text](<../Images/Capture d'écran 2025-12-28 161122.png>)

:gear: paramétrage du module et récupération des comptes avec Metasploit:
![alt text](<../Images/Capture d'écran 2025-12-28 161649.png>)

:bulb: **Alternative** : Récupération des credentials avec Hydra :
![alt text](<../Images/Capture d'écran 2025-12-28 164512.png>)
:gear: **Enumération des partages SMB**
![alt text](<../Images/Capture d'écran 2025-12-28 163248.png>)

:bulb: **Alternative** avec `crackmapexec` pour tester la connexion et enumérer les partages  
![alt text](<../Images/Capture d'écran 2025-12-28 171907.png>)

:gear: Connexion avec `smbclient` car nous avons les credentials et les partages (partage par défaut : C$)  
![alt text](<../Images/Capture d'écran 2025-12-28 170622.png>)
![alt text](<Capture d'écran 2025-12-28 170757.png>)

:bulb: Alternative avec `psexec` sur Metasploit
![alt text](<../Images/Capture d'écran 2025-12-28 170128.png>)
![alt text](<../Images/Capture d'écran 2025-12-28 165650.png>)

**Flag3 PWNED !**  

</details>

---

<details>

<summary><h3> :arrow_forward: Flag4 Le répertoire Bureau contient peut-être ce que vous cherchez. Parcourez son contenu. (target2.ine.local)</summary><h3> 

❕Aller dans **Users>Administrator>Desktop** et récupérer le flag4.
![alt text](<../Images/Capture d'écran 2025-12-28 172509.png>)

:bulb: **Alternative** avec `exploit/windows/smb/psexec` flag4.txt**  
Se déplacer dans "Users>Administrator>Desktop"
![alt text](<../Images/Capture d'écran 2025-12-28 172915.png>)
**Flag 4 PWNED!**
</details>
