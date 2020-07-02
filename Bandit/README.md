
### Level 0
 ``` 
    $ ssh -p2220 bandit0@bandit.labs.overthewire.org 
    $ cat readme 
```

 **pwd** : ```boJ9jbbUNNfktd78OOpsqOltutMc3MY1```

 <hr>

### Level 1

 ``` 
    $ ssh -p2220 bandit1@bandit.labs.overthewire.org 
    $ cat ./-
```

Here, the file is '-'. Now simply typing ```cat -``` will not give the desired output because cat treats it as a synonym for stdin. So we can use it in a slight different manner and use
``` cat ./-``` instead. 

 **pwd** : ```CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9```

 <hr>

### Level 2

 ``` 
    $ ssh -p2220 bandit2@bandit.labs.overthewire.org 
    $ cat spaces\ in\ this\ filename
```

The filename here had spaces between them, so while using cat we need to use backslash as an escape sequence. Much easier way to get around this is type a first few chars and then press tab for autocompletion, it takes care of that itself.

 **pwd** : ```UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK```

<hr>

### Level 3

 ``` 
    $ ssh -p2220 bandit3@bandit.labs.overthewire.org 
    $ cd inhere
    $ ls -a
```

Hidden files in linux system are prefixed with '.' . Just ls command won't show the hidden files. Use ls -a instead.

 **pwd** : ```pIwrPrtPN36QITSp3EQaw936yaFoFgAB```

<hr>

### Level 4

 ``` 
    $ ssh -p2220 bandit4@bandit.labs.overthewire.org 
    $ for((i=0; i<=9; i++)); do strings ./-file0$i ; done
```

To get the human readable chars we can simply use the ```strings``` command. Here we have 10 files we can manually run strings command against each of them or simply run a oneliner bash loop. To learn more click [here](https://www.cyberciti.biz/faq/bash-for-loop/)

 **pwd** : ```koReBOKuIDDepwhWk7jZC0RTdopnAYKh```

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

 **pwd** : ```DXjZPULLxYr17uwoI01bNLQbtFemEgo7```

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

 **pwd** : ```HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs```

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

 **pwd** : ```cvX2JJa4CFALtqS87jk27qwqGhBM9plV```

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

**pwd** : ```UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR```

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

**pwd** : ```truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk```

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

**pwd** : ```IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR```

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

**pwd** : ```5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu```

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

**pwd** : ```8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL```

<hr>

### Level 13

#### Question

The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on

#### Answer
 ``` 
    $ ssh -p2220 bandit13@bandit.labs.overthewire.org
    $ ssh bandit14@localhost -i sshkey.private 
```

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

**pwd** : ```BfMYroe26WYalil77FoDi9qh59eK5xNr```

<hr>

### Level 15

#### Question

The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.

Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…

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

**pwd** : ```5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu```

<hr>


References:
* [CheatSheet](https://gist.github.com/bradtraversy/ac3b1136fc7d739a788ad1e42a78b610#file-myscript-sh)