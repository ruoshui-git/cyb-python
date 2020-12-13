# Problem with John

Default installation of `john` always fail:

```
┌──(known㉿DESKTOP-DTPC4J5)-[~]
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt sshash 
[SSH] cipher value of 6 is not supported!
Using default input encoding: UTF-8
No password hashes loaded (see FAQ)
```

# Build John from source

First, clone john

$ `git clone -depth 1 https://github.com/openwall/john.git`

Install clang b/c I don't have it. GCC should be fine too.

$ `sudo apt-get install clang`

I don't have OpenSSL header. This is needed. Install.

$ `sudo apt install libssl-dev`

In `john/src`:

$ `./configure`

Last few lines of output:
```
Configured for building John the Ripper jumbo:

Target CPU ................................. x86_64 AVX2, 64-bit LE
AES-NI support ............................. depends on OpenSSL
Target OS .................................. linux-gnu
Cross compiling ............................ no
Legacy arch header ......................... x86-64.h

Optional libraries/features found:
Memory map (share/page large files) ........ yes
Fork support ............................... yes
OpenMP support ............................. yes (not for fast formats)
OpenCL support ............................. no
Generic crypt(3) format .................... yes
libgmp (PRINCE mode and faster SRP formats)  no
128-bit integer (faster PRINCE mode) ....... yes
libz (pkzip and some other formats) ........ no
libbz2 (gpg2john extra decompression logic)  no
libpcap (vncpcap2john and SIPdump) ......... no
OpenMPI support (default disabled) ......... no
ZTEX USB-FPGA module 1.15y support ......... no

Install missing libraries to get any needed features that were omitted.

Configure finished.  Now "make -s clean && make -sj4" to compile.
```
Now install:

```
┌──(known㉿DESKTOP-DTPC4J5)-[~/cyber/john/src]
└─$ make -s clean && make -sj4
DES_bs.c:501:25: warning: variable 't' is uninitialized when used here [-Wuninitialized]
                b = (DES_bs_vector *)&DES_bs_all.B[0] DEPTH;
                                      ^~~~~~~~~~

... Lots of warnings by clang ...

ar: creating aes.a
ar: creating secp256k1.a
ar: creating ed25519-donna.a

Make process completed.
```

Now, in `../run`:

```
┌──(known㉿DESKTOP-DTPC4J5)-[~/cyber/john/run]
└─$ ./john
John the Ripper 1.9.0-jumbo-1+bleeding-84d7390 2020-12-12 13:43:56 +0100 OMP [linux-gnu 64-bit x86_64 AVX2 AC]
Copyright (c) 1996-2020 by Solar Designer and others
Homepage: https://www.openwall.com/john/

Usage: john [OPTIONS] [PASSWORD-FILES]

Use --help to list all available options.
```

Cracking:

```
┌──(known㉿DESKTOP-DTPC4J5)-[~/cyber/john/run]
└─$ ./john --wordlist=/usr/share/wordlists/rockyou.txt ~/sshash
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 2 for all loaded hashes
Cost 2 (iteration count) is 16 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
password123      (id_rsa)
1g 0:00:01:23 DONE (2020-12-12 11:39) 0.01192g/s 16.78p/s 16.78c/s 16.78C/s jesse..tagged
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```