Websites track the website we are comming from by the **HTTP Referer** headers. And somehow we need to change our referer header to `http://natas5.natas.labs.overthewire.org/`. But we cannot do that. What we can do is redirect the current website from this `http://natas5.natas.labs.overthewire.org/`. How? By using JS. HOWWWW?

Just change the `location.href`. 

1. Visit `http://natas5.natas.labs.overthewire.org/`. It will ask for password cancel those. 
2. Open console and write `window.location.href="http://natas4.natas.labs.overthewire.org/"`.

You will be redirected back this time the referrer is changed, and you will get the passwd

**passwd :** `iX6IOfmpN7AYOQGPwtn3fXpbaJVJcHfq`