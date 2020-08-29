We can view the source. 

```php
include "includes/secret.inc";

    if(array_key_exists("submit", $_POST)) {
        if($secret == $_POST['secret']) {
            print "Access granted. The password for natas7 is <censored>";
        } else {
            print "Wrong secret";
        }
    }
```

One of the very first thing we notice is that it includes a file `secret.inc`. So, let's try if its public and voila!. It is public. Just going to that file `http://natas6.natas.labs.overthewire.org/includes/secret.inc` We can see the value of the $secret. Put that in the input and you will get the passwd

**passwd :** `7z3hEENjQtflzgnT29q7wAvMNfZdh0i9`