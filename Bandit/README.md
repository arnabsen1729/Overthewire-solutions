
### Level 0
 ``` 
    $ ssh -p2220 bandit0@bandit.labs.overthewire.org 
    $ cat readme 
```

 **passwd** : ```boJ9jbbUNNfktd78OOpsqOltutMc3MY1```

 <hr>

### Level 1

 ``` 
    $ ssh -p2220 bandit1@bandit.labs.overthewire.org 
    $ cat ./-
```

Here, the file is '-'. Now simply typing ```cat -``` will not give the desired output because cat treats it as a synonym for stdin. So we can use it in a slight different manner and use
``` cat ./-``` instead. 

 **passwd** : ```CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9```

 <hr>

### Level 2

 ``` 
    $ ssh -p2220 bandit2@bandit.labs.overthewire.org 
    $ cat spaces\ in\ this\ filename
```

The filename here had spaces between them, so while using cat we need to use backslash as an escape sequence. Much easier way to get around this is type a first few chars and then press tab for autocompletion, it takes care of that itself.

 **passwd** : ```UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK```

<hr>

### Level 3

 ``` 
    $ ssh -p2220 bandit3@bandit.labs.overthewire.org 
    $ cd inhere
    $ ls -a
```

Hidden files in linux system are prefixed with '.' . Just ls command won't show the hidden files. Use ls -a instead.

 **passwd** : ```pIwrPrtPN36QITSp3EQaw936yaFoFgAB```

<hr>

### Level 4

 ``` 
    $ ssh -p2220 bandit4@bandit.labs.overthewire.org 
    $ for((i=0; i<=9; i++)); do strings ./-file0$i ; done
```

To get the human readable chars we can simply use the ```strings``` command. Here we have 10 files we can manually run strings command against each of them or simply run a oneliner bash loop. To learn more click [here](https://www.cyberciti.biz/faq/bash-for-loop/)

 **passwd** : ```koReBOKuIDDepwhWk7jZC0RTdopnAYKh```

<hr>

### Level 5

#### Question

The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

* human-readable
* 1033 bytes in size
* not executable

#### Answer
 ``` 
    $ ssh -p2220 bandit5@bandit.labs.overthewire.org 
    $ cd inhere
```

Listing all the folders in the inhere directory we see there are a lot of them. So smart move would be to use the conditions in the question as search parameters. For more reference to find command type ```$ man find``` in the terminal.

```
    $ strings "$(find . -size 1033c)"
```

Let's explain this command, inside the quotes a $ sign is mentioned and then inside brackets a bash command. So the bash terminal runs the bash command, and then whatever output it gets it sends that to the strings command.

 **passwd** : ```DXjZPULLxYr17uwoI01bNLQbtFemEgo7```

 <hr>

### Level 6

#### Question

The password for the next level is stored somewhere on the server and has all of the following properties:

 * owned by user bandit7
 * owned by group bandit6
 * 33 bytes in size

#### Answer
 ``` 
    $ ssh -p2220 bandit6@bandit.labs.overthewire.org 
    $ cd /
```

Simply use the params in the find command, you will notice most of the files are *Permission Denied* except one. Just strings or cat that file

```
    $ find . -depth -size 33c -group bandit6 -user bandit7
    $ strings ./var/lib/dpkg/info/bandit7.password
```

Now it might be tough to locate that file among all the errors so we can simply redirect those errors. We can use ```2> /dev/null``` to achieve that. For more on that read [this](https://askubuntu.com/questions/350208/what-does-2-dev-null-mean)

So this will do the magic. 

```
    $strings "$(find . -depth -type f  -size 33c -group bandit6 -user bandit7 2> /dev/null)"
```

 **passwd** : ```HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs```

 <hr>

### Level 7

#### Question

The password for the next level is stored in the file data.txt next to the word millionth

#### Answer
 ``` 
    $ ssh -p2220 bandit7@bandit.labs.overthewire.org 
```
So simply grep would achive that. So just use strings on data.txt and pipe that to grep 

```
    $ strings data.txt | grep millionth
```

 **passwd** : ```cvX2JJa4CFALtqS87jk27qwqGhBM9plV```

 <hr>

### Level 8

#### Question

The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

#### Answer
 ``` 
    $ ssh -p2220 bandit8@bandit.labs.overthewire.org 
```
First learn about sort and uniq commands, read the manual for them. It is just using the correct flags. 

```
    $ sort data.txt | uniq -u
```

**passwd** : ```UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR```

<hr>

### Level 9

#### Question

The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

#### Answer
 ``` 
    $ ssh -p2220 bandit9@bandit.labs.overthewire.org 
```
Since it says human-readable we will use strings, and then pipe the output through grep =

```
    $ strings data.txt | grep ==
```
You will get an output something like this
```
========== the*2i"4
========== password
Z)========== is
&========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
```
Clearly we know which one the passwd is 

**passwd** : ```truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk```

<hr>

### Level 10

#### Question

The password for the next level is stored in the file data.txt, which contains base64 encoded data

#### Answer
 ``` 
    $ ssh -p2220 bandit10@bandit.labs.overthewire.org 
    $ cat data.txt
    VGhlIHBhc3N3b3JkIGlzIElGdWt3S0dzRlc4TU9xM0lSRnFyeEUxaHhUTkViVVBSCg==
```
Read about [base64](https://en.wikipedia.org/wiki/Base64). There are many ways to decode it

1. Use an online decoder like base64decode.org (not recommended)
2. Use python3: There is a library in python called base64 which can do it. So 
```
    $ python3 #use the interpreter
    >>> import base64
    >>> base64.b64decode('VGhlIHBhc3N3b3JkIGlzIElGdWt3S0dzRlc4TU9xM0lSRnFyeEUxaHhUTkViVVBSCg==')
    b'The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR\n'
```
3. Use the bash.

```
    $ cat data.txt | base64 -d
```

**passwd** : ```IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR```

<hr>

### Level 11

#### Question

The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

#### Answer
 ``` 
    $ ssh -p2220 bandit11@bandit.labs.overthewire.org 
```
It is one of the basic ciphers known as Caesar Cipher or ROT13. 
There is a website called rot13.org which decodes it. Otherwise we can use the tr command to do it in the terminal. Again to know more about tr command type ```man tr``` in terminal.

```
    $ cat data.txt | tr '[A-MN-Za-mn-z]' '[N-ZA-Mn-za-m]'
```
tr can map of set of chars with another set. That is what is used here

**passwd** : ```5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu```

<hr>

### Level 12

#### Question

The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

#### Answer
 ``` 
    $ ssh -p2220 bandit12@bandit.labs.overthewire.org 
```

It was one of the most tedious task so far. Always use file command to check what sort of file it is. And make sure you rename the file to have the correct suffix (or extension).

As suggested I created a folder in tmp. I made abc19, if the name you chose is already present choose a different name.

```
    $ mkdir /tmp/abc19
    $ cp data.txt /tmp/abc19
```
1. We have a hex dump, we can reverse that i.e get the binary by using ```xxd``` .
```
    $ xxd -r data.txt bin
```
2. The bin file is a gzip compressed data and to decompress it must have a suffix of gz.

```
    $ mv bin bin.gz
    $ gzip -d bin.gz
```

3. The bin file so created is a bzip2 compressed file. Again decompress it but change the suffix

```
    $ mv bin bin.bz2
    $ bzip2 -d bin.bz2
```

4. The bin file is again gzip compressed. 

```
    $ mv bin bin.gz
    $ gzip -d bin.gz
```

5. Then we get a tar file. x-> extract, v-> verbose

```
    $ tar -xvf bin
```

6. Another file which is again a tar file. 
```
    $ tar -xvf data5.bin
```

7. We have to keep identifying the file type and undergo decompression.

```
    $ mv data6.bin data6.bz2
    $ bzip2 -d data6.bz2
    $ tar -xvf data6
    $ mv data8.bin data8.gz
    $ gzip -d data8.gz
    $ cat data8
    The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL

```
Ahh! Finally 

**passwd** : ```8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL```

<hr>

### Level 13

#### Question

The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on

#### Answer
 ``` 
    $ ssh -p2220 bandit13@bandit.labs.overthewire.org
    $ ssh bandit14@localhost -i sshkey.private 
```

Now since you are logged in as bandit14 we can get the pass by opening the file `/etc/bandit_pass/bandit14`

**passwd** : ```4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e```

<hr>

### Level 14

#### Question

The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

#### Answer

Passwords are located in the etc/bandit_pass/ . So we can get the pass of this level.

 ``` 
    $  cat /etc/bandit_pass/bandit14
    4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
```

Then use that pass to submit it to localhost at port 30000. We can use netcat for that. 

```
    $ echo "4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e" | nc localhost 30000
    Correct!
    BfMYroe26WYalil77FoDi9qh59eK5xNr
```

**passwd** : ```BfMYroe26WYalil77FoDi9qh59eK5xNr```

<hr>

### Level 15

#### Question

The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.

Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…

#### Answer
 ``` 
    $ ssh -p2220 bandit15@bandit.labs.overthewire.org 
```
Earlier since we had no ssl encryption needed we used `netcat`, now we have to use SSL encryption for that we will use `openssl`. The syntax being provided in the references. 
```
   $ openssl s_client -connect localhost:30001
```
After this command we will have to enter the passwd of this level and we will get the passwd for the next one. Also we can do this in one go by

```
    $ cat /etc/bandit_pass/bandit15 | openssl s_client -connect localhost:30001 -quiet
```

**passwd** : ```cluFn7wTiGryunymYOu4RcffSxQluehd```

<hr>

### Level 16

#### Question

The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

#### Answer

```
    $ ssh -p2220 bandit16@bandit.labs.overthewire.org
```
Since we have to scan ports, the tool that comes in our mind is `nmap`. First let's fire up nmap giving it a port range and in verbose mode and let's see what we get

```
  $ nmap -v localhost -p31000-32000
```

The output we get:

```
Scanning localhost (127.0.0.1) [1001 ports]
Discovered open port 31960/tcp on 127.0.0.1
Discovered open port 31691/tcp on 127.0.0.1
Discovered open port 31518/tcp on 127.0.0.1
Discovered open port 31790/tcp on 127.0.0.1
Discovered open port 31046/tcp on 127.0.0.1
Completed Connect Scan at 09:57, 0.05s elapsed (1001 total ports)
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00048s latency).
Not shown: 996 closed ports
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown
```
But this doesn't give us any info about the SSL encryption. One way would be to try all the 5 ports but a smarter approach would be to determine the service and the version in these port. And for that we can use the `-sV` flag of nmap.

```
  $ nmap -v -sV localhost -p31000-32000
```

Now the output is:

```
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port31790-TCP:V=7.40%T=SSL%I=7%D=8/23%Time=5F4221B5%P=x86_64-pc-linux-g
SF:nu%r(GenericLines,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20cu
SF:rrent\x20password\n")%r(GetRequest,31,"Wrong!\x20Please\x20enter\x20the
SF:\x20correct\x20current\x20password\n")%r(HTTPOptions,31,"Wrong!\x20Plea
SF:se\x20enter\x20the\x20correct\x20current\x20password\n")%r(RTSPRequest,
SF:31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x20password\
SF:n")%r(Help,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x
SF:20password\n")%r(SSLSessionReq,31,"Wrong!\x20Please\x20enter\x20the\x20
SF:correct\x20current\x20password\n")%r(TLSSessionReq,31,"Wrong!\x20Please
SF:\x20enter\x20the\x20correct\x20current\x20password\n")%r(Kerberos,31,"W
SF:rong!\x20Please\x20enter\x20the\x20correct\x20current\x20password\n")%r
SF:(FourOhFourRequest,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20c
SF:urrent\x20password\n")%r(LPDString,31,"Wrong!\x20Please\x20enter\x20the
SF:\x20correct\x20current\x20password\n")%r(LDAPSearchReq,31,"Wrong!\x20Pl
SF:ease\x20enter\x20the\x20correct\x20current\x20password\n")%r(SIPOptions
SF:,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x20password
SF:\n");
```

Now we are sure that its port 31790 which is running ssl and not echo. So we can try connecting to it. 

```
    $ cat /etc/bandit_pass/bandit16 | openssl s_client -connect localhost:31790 -quiet
```

And the output is a private SSH key:

```
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----
```

For logging in as bandit17 we need to save this RSA key in a file (eg. otw). Remember ssh directory permissions should be 700 (drwx------). The public key (. pub file) should be 644 (-rw-r--r--). The private key (id_rsa) on the client host, and the authorized_keys file on the server, should be 600 (-rw-------). 

So give 600 perms to the private key file. 

```
    $ chmod 600 otw
    $ ssh -p2220 bandit17@bandit.labs.overthewire.org -i otw
```

<hr>

### Level 17

#### Question

There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19

#### Answer

Clearly we have to use the `diff` command.

```
    $ bandit17@bandit:~$ diff passwords.new passwords.old
      42c42
      < kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
      ---
      > w0Yfolrc5bwjS4qw5mq1nnQi6mF03bii

```

And we get the passwd

**passwd**: `kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd`

<hr>

### Level 18

#### Question
The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

#### Answer

First, when we try to login we are forced to logout as mentioned in the question. But we can pass commands along with the ssh command. Read more [here](https://www.cyberciti.biz/faq/unix-linux-execute-command-using-ssh/). So we will do that.

```
    $ ssh -p2220 bandit18@bandit.labs.overthewire.org 'cat ./readme'
```

This will spit out the passwd for the next level. 

**passwd*: `IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x`

<hr>


### Level 19

#### Question
To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

#### Answer

```
    $ ssh -p2220 bandit19@bandit.labs.overthewire.org
```

To get some basic idea about setuid, guid and sticky bit watch this video [here](https://www.youtube.com/watch?v=2gHp_CgUets). And as the question suggests when I run the command `./bandit20-do whoami` it says `bandit20` so like whatever we make it execute will be executed with the perms of `bandit20` and thus we can easily look into the passwd.

```
    $ ./bandit20-do cat /etc/bandit_pass/bandit20
```

This will spit out the passwd for the next level. 

**passwd*: `GbKksEFF4yrVs6il55v6gwY5aVje5f0j`

<hr>

### Level 20

#### Question
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

#### Answer

```
    $ ssh -p2220 bandit21@bandit.labs.overthewire.org
```

Basically what we need to do is set up our own port which will echo out the current password. I have taken 4242 as just an example. 

```
    $ echo "GbKksEFF4yrVs6il55v6gwY5aVje5f0j" | nc -l localhost -p 4242
```

But you will notice one thing once you do this, you cannot type any other command so you need to run it in background. In linux we can do that by just appending a '&' at the end. It will run this process in the background. 

```
    $ echo "GbKksEFF4yrVs6il55v6gwY5aVje5f0j" | nc -l localhost -p 4242 &
    $ ./suconnect 4242
```

This will give us the passwd


**passwd*: `gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr`


<hr>

### Level 21

#### Question
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

#### Answer

```
    $ ssh -p2220 bandit21@bandit.labs.overthewire.org
```

Just follow the steps. 
1. List all the files in the cron folder. `cd /etc/cron.d && ls`
2. The file that is in our interest is the `cronjob_bandit22`. So cat the file. 
3. You will see it is doing something with this script `/usr/bin/cronjob_bandit22.sh`. So check the script out

The script has this
```bash
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

So the pass for the level is copied in that temp file. So simply cat the file

```
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
This wil give the passwd

**passwd*: `Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI`

<hr>

### Level 22

#### Question
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints..

#### Answer

```
    $ ssh -p2220 bandit22@bandit.labs.overthewire.org
```

We have to follow the same steps as previous question. Do 1, 2, 3 of the above question. And now check out the script. 

```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

Now this time too the password of the current level is copied to a file in target which is calculated. Now a easier solution would be to create a bash executable of our own that will generate the `mytarget`. 
So in the `/tmp` folder create a bash file. And save this in that file. 

```bash
#!/bin/bash

myname=bandit23
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

```
Now one thing if you look into the perms of the bash file you will notice that the file belongs to `bandit23` so the result of the `whoami` will be `bandit23`.
Make this file executable and run it 

```
Copying passwordfile /etc/bandit_pass/bandit23 to /tmp/8ca319486bfbbc3663ea0fbe81326349
```
Then again cat the temp file to get the passwd

```
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
> jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n
```


**passwd*: `jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n`

<hr>

### Level 23

#### Question
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

#### Answer

```
    $ ssh -p2220 bandit23@bandit.labs.overthewire.org
```

Again following those steps 1, 2 find the script that is run by cron. 
```bash
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname
echo "Executing and deleting all scripts in /var/spool/$myname:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

What this script does is, it goes through every executable in the folder `/var/spool/$myname` and then executes them and after that deletes them. Now our objective is to get the pass for the next level i.e bandit24. So we can write a script that will fetch that password and store it somewhere. And since we can't run that script we will make this cron do that for us. 

So let's write a script. Now we can access only the `/tmp/` of the server. So let's write one bash there. 

```
$ vim myscript.sh
```

and inside that script write this.

```bash
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/lvl24pass
```

now copy this script to our destination folder but before that make it executable.

```bash
chmod +x ./myscript.sh
cp ./myscript.sh /var/spool/bandit24/
```

But the cron file had some caveats. 
`owner="$(stat --format "%U" ./$i)"` Gives us the owner of the file and since we are bandit23 so it will pass without any difficulty. 

Now wait for 1min approx. And run

```
cat /tmp/lvl24pass
```
If it say's `file not found` wait for a couple of secs more. Otherwise it would give us the pass


**passwd*: `UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ`

<hr>

### Level 24

#### Question
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.

#### Answer

```
    $ ssh -p2220 bandit24@bandit.labs.overthewire.org
```

The question is pretty straight forward. There is a daemon(bg process) running on 30002. Try `nc localhost 30002` you will see it says,

>I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.

So now we have to enter the pass of the level 24 and a pin. And we need to bruteforce the pincode. 

You can try using a bash script or a python script. At first my try was to use `subprocess` and run the command ` cat password+pin | nc localhost 3002` but there was a problem, it was very slow, because we are making a connection and closing a connection every second. But what I noticed is that if we give a wrong input the connection is not closed, so I should rather use something like socket in python to make a one time connection and then keep feeding data. 

If you are new to socket go through the docs or this [GFG article](https://www.geeksforgeeks.org/socket-programming-python/)

So using this we will create a socket and connect it to localhost and port 30002. Bruteforce all the pins in the format asked, and check the response. It will take a few seconds because in my case 2588 was the correct pin. And along with that there was the password . 

Here is my script.

```python

import socket

s = socket.socket()
s.connect(('127.0.0.1', 30002))
print('Socket Connected')
s.recv(1024)

for pin in range(10000):
    data = 'UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ ' + str(pin).zfill(4)+'\n'
    s.sendall(data.encode())
    print('PIN: ', str(pin).zfill(4), end=' ')
    response = s.recv(1024).decode()

    if "Wrong" in response:
        print('WRONG')
    else:
        print('CORRECT')
        print(response)
        break


```

**passwd*: `uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG`

References:
* [CheatSheet](https://gist.github.com/bradtraversy/ac3b1136fc7d739a788ad1e42a78b610#file-myscript-sh)
                                                                       
