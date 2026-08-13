# Fawn Box Notes

##  Enumeration
```
This box is about FTP port, so ill focus my port scan to just that
```
Ran command sudo nmap -sC -sV -p 21 10.129.49.107 -oX Fawn.xml -oN Fawn.txt

Found something interesting, **ftp-anon** is set to 1, going to attempt to connect with the user anonymous

That worked! Next part

## Exploitation
Connected to FTP, remember to set to passive!

ls shows 
```
ftp> ls
227 Entering Passive Mode (10,129,49,107,177,201).
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
```

Now ill download the txt using GET
