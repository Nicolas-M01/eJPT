Utiliser par ex `auxiliary/scanner/ssh/ssh_login`, une fois les creds trouvés, upgrader la session.  
Puis `use exploit/unix/local/chrootkit` nous permet d'élever nos droits.  
`post/linux/manage/sshkey_persistance`, rentrer la sessions meterpreter (root), le CREASTESSHFOLDER, puis run. On doit avoir une clé privée de générée. On copie colle dans un fichier "ssh_key". Les droits à "400". puis on se connecte avec cette clé ssh sur root : `ssh -i ssh_key root@ip`  
