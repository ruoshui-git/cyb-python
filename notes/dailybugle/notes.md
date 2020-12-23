# Finding out Joomla Version
https://www.itoctopus.com/how-to-quickly-know-the-version-of-any-joomla-website

# Joomla sqli exploit
https://www.exploit-db.com/exploits/42033

I ran the command on the exploit page. It worked.


Hashcat hash type 
```
 3200 | bcrypt $2*$, Blowfish (Unix)                     | Operating System
```

joomla:

jonah:$2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm


Grant remote access mysql:

https://stackoverflow.com/a/1559992/7584881

Exploit mysql only if it's running as root 

https://recipeforroot.com/mysql-to-system-root/

Password for mysql database is also for jjameson... So I never needed a reverse shell. I saw the content when I was exploring the fs with the ProFile Joomla plugin.

And of course the user flag is in $HOME of jjameson.

[Privilege escalation](https://gtfobins.github.io/gtfobins/yum/) at this stage is very easy. But `sudo -l` helps by showing that `yum` is allowed.