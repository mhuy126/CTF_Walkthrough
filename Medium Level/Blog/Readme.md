# Blog

> ************************Instructions************************
> 
> 
> Billy Joel made a blog on his home computer and has started working on it.  It's going to be so awesome!
> 
> Enumerate this box and find the 2 flags that are hiding on it!  Billy has some weird things going on his laptop.  Can you maneuver around and get what you need?  Or will you fall down the rabbit hole...
> 
> *In order to get the blog to work with AWS, you'll need to add blog.thm to your /etc/hosts file.*
> 
> *Credit to [Sq00ky](https://tryhackme.com/p/Sq00ky) for the root privesc idea ;)*
> 

### Hints/Keys
- Linux
- SMB: `smbclient`, `smbmap`
- Wordpress: `wpscan` - enum + brute-force password
- Reverse shell: Metasploit (`msfconsole`)
- Privilge Escalation: exploit binary through `ghidra`