## Question

The first level is easy.  The password for level 2 is in the file 
'krypton2'.  It is 'encrypted' using a simple rotation called ROT13.  
It is also in non-standard ciphertext format.  When using alpha characters for
cipher text it is normal to group the letters into 5 letter clusters, 
regardless of word boundaries.  This helps obfuscate any patterns.

This file has kept the plain text word boundaries and carried them to
the cipher text.

## Answer

We can do this in many different ways, one approach would be to use `tr` command. According to man page

**tr**
>Translate, squeeze, and/or delete characters from standard input, writing to standard output.

`tr` works basically like mapping, you give one charset and the final charset you want to map it to.
In this case we are given ROT13 so A will map to N, B-O and so on.

```bash
cat krypton2 | tr A-Z N-ZA-M
```

This is it. This gives us the password

`ROTTEN`