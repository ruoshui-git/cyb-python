# Steps

1. Nmap scan:

```
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http
```

2. ftp login (anonymous)

3. lock.txt is the password list; usename is in the other file.

4. hydra ssh

5. sudo -l shows `tar` is a command that the user can run
    - `tar` can do lots of things, like zipping your whole filesystem perhaps

Or, as I learned later, `tar` can be used to do privilege escalation. Or a whole bunch of other things: https://gtfobins.github.io/gtfobins/tar/
