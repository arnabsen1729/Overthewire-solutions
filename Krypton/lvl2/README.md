## Question

The password for level 3 is in the file krypton3. It is in 5 letter group ciphertext. It is encrypted with a Caesar Cipher. Without any further information, this cipher text may be difficult to break. You do not have direct access to the key, however you do have access to a program that will encrypt anything you wish to give it using the key. If you think logically, this is completely easy.

## Answer

let's create a folder in temp to work with. 

```bash

mkdir /tmp/abcabc
cd /tmp/abcabc
```

Now let's link ta keyfile to our directory

```basj
ln -s /krypton/krypton2/keyfile.dat
chmod 777 .
```

Now everythime we execute the encrypt on a file it shifts (ecrypts) the text in the file and exports the result as a 'ciphertext' file. Since the key file cannot be changes or seen, we can be sure eveytime it shifts the delta will be same i.e the difference by which the shift occurs will remain the same. Also we can read the `ciphertext` files it generates so let's try. 

First create a custome Alphabet set. For convinience let it be `ABCDEFGHIJKLMNOPQRSTUVWXYZ`

```bash
echo 'ABCDEFGHIJKLMNOPQRSTUVWXYZ' > letters
/krypton/krypton2/encrypt ./letters
cat ciphertext
>MNOPQRSTUVWXYZABCDEFGHIJKL
```

So now we know what set maps to what, so just put those char sets in tr

```bash
cat /krypton/krypton2/krypton3 | tr MNOPQRSTUVWXYZABCDEFGHIJKL A-Z
```

Ans you will get the answer

```CAESARISEASY```

If you don't want to work so hard, write a script that will generate all the ROTs 

```bash
charset=ABCDEFGHIJKLMNOPQRSTUVWXYZ 
input=${1,,}

for char in $(seq 1 ${#charset}); do 
  temp=${charset#?} 
  charset=${temp}${charset%"$temp"} 
  echo $input | tr a-z $charset
done
```
```bash
./rotn.sh 'OMQEMDUEQMEK'
PNRFNEVFRNFL
QOSGOFWGSOGM
RPTHPGXHTPHN
SQUIQHYIUQIO
TRVJRIZJVRJP
USWKSJAKWSKQ
VTXLTKBLXTLR
WUYMULCMYUMS
XVZNVMDNZVNT
YWAOWNEOAWOU
ZXBPXOFPBXPV
AYCQYPGQCYQW
BZDRZQHRDZRX
CAESARISEASY <---
DBFTBSJTFBTZ
ECGUCTKUGCUA
FDHVDULVHDVB
GEIWEVMWIEWC
HFJXFWNXJFXD
IGKYGXOYKGYE
JHLZHYPZLHZF
KIMAIZQAMIAG
LJNBJARBNJBH
MKOCKBSCOKCI
NLPDLCTDPLDJ
OMQEMDUEQMEK
```