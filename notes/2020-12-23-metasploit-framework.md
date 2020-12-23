# Installing and Using Metasploit Framework on WSL2 Kali

**Important:** You need Windows version 2004 or higher AND wsl 2 to do this.

## Install

First we need the metasploit framework (we'll call it "msf"):

$ `sudo apt install metasploit-framework`


It's big. It will take some time.

## Intialize Database

The first time we use msf, we need to inititialize the database. Msf uses postgresql.

The command `sudo msfdb init` will not work, because it uses systemctl to start postgresql, but that doesn't work in WSL2 yet, since systemd doesn't start with a PID 1. 

So, first, make sure postgresql is installed with [Microsft's instruction](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-database).

Start postgresql service with

$ `sudo service postgresql start`

Initialize database, if you haven't already.

$ `sudo msfdb init`

If you have already initialized database got failed, run:

$ `sudo msfdb reinit`

You should see something like:
```
[+] Creating database user 'msf'

[+] Creating databases 'msf'

[+] Creating databases 'msf_test'

[+] Creating configuration file '/usr/share/metasploit-framework/config/database.yml'

[+] Creating initial database schema
```

## Using Metasploit

Now start msf and use as normal

$ `msfconsole`

Verify that database is connected:

```
msf6 > db_status

[*] Connected to msf. Connection type: postgresql.

msf6 >
```

## Using meterpreter and VPN (the tryhackme setup)

There's an issue about using VPN in WSL2. Meterpreter will fail if the problem isn't solved.

For me, (doing tryhackme), this line solved the problem after connecting with OpenVPN in wsl.

$ sudo sysctl -w net.ipv4.tcp_mtu_probing=1

See [this GitHub issue](https://github.com/microsoft/WSL/issues/4698) and [this comment](https://github.com/microsoft/WSL/issues/4698#issuecomment-712890576) for more details.