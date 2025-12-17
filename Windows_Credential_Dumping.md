

Windows OS range les hashes des passwords des utilisateurs dans la database SAM (Security Accounts Manager)  
Un hash est le procédé de convertir une donnée en une autre valeur. Une fonction de hashage ou algorythme est utilisé pour générer une nouvelle valeur. Le résultat est le hash.  
L'authentification et la vérification des credentials est facilité par Local Security Authority (LSA).  
Windows jusqu'aux version Win Serv 2003 utilisait 2 différents types de hashes :  
* LM  
* NTLM  
  
LM a été arrêté utilise les hashes NTLM depuis VISTA.  

## SAM
* SAM (Security Account Manager) est une base de données Windows qui gère les comptes utilisateurs et leurs mots de passe, stockés sous forme hachée.  
* Le fichier SAM est verrouillé pendant que Windows fonctionne, donc il ne peut pas être copié directement.  
* Le noyau Windows NT protège le SAM ; pour cette raison, les attaques ciblent souvent la mémoire, notamment le processus LSASS, afin d’extraire les hash.  
* Dans les versions modernes de Windows, le SAM est chiffré à l’aide d’une clé système (syskey).  
* Des privilèges administrateur/élevés sont nécessaires pour accéder au processus LSASS.  
👉 En bref : Windows protège fortement les mots de passe via le SAM, le chiffrement et le verrouillage mémoire, et leur accès nécessite des droits élevés.  

## LM (LanMan)  
* LM (LAN Manager) est un ancien algorithme de hachage utilisé par Windows avant NT 4.0.  
* Le mot de passe est :  
  * découpé en deux blocs de 7 caractères,  
  * converti en majuscules,  
  * puis chaque bloc est haché séparément avec DES.  
* LM est très faible en sécurité : absence de sel (salt), découpage prévisible et DES rendent les mots de passe faciles à casser par force brute ou tables arc-en-ciel.  
👉 En bref : LM est obsolète et dangereux, raison pour laquelle il est désactivé sur les systèmes Windows modernes.  

## NTLM (NTHash) 
* NTLM (NTHash) est un ensemble de protocoles d’authentification utilisés par Windows pour vérifier l’identité des utilisateurs entre machines.  
* À partir de Windows Vista, le hachage LM est désactivé au profit de NTLM.  
* Le mot de passe utilisateur est haché avec l’algorithme MD4, puis le mot de passe en clair est supprimé.  
* NTLM améliore LM car il :  
  * ne découpe pas le mot de passe en blocs,  
  * est sensible à la casse,  
  * supporte les symboles et caractères Unicode.  
👉 En bref : NTLM est plus sécurisé que LM, mais reste aujourd’hui moins robuste que les mécanismes modernes (ex. Kerberos).  

