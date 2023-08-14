# Wonderland

### Hints/Keys
- Image analyzing: `exiftool`, `steghide`
- Multiple Privilege Escalation steps:
	+ `sudo -l`, *import module* exploit
	+ binary called exploit: `date`, `PATH`
	+ **Linux Capabilities**: `getcap`, `find / -group <grouid> 2>/dev/null`