# InfosecPrep

### port enumeration :
first thing to start with is the is port descovery, soo i should run nmap to detect which ports are openned

```sh
 nmap -sC -sV -p- <ip>
```

- -sC to run standard script 
- -sV to get service version
- -p- for all ports
  
![initial enomuration image](https://github.com/FR1NG/oscp/blob/master/screenshots/InfosecPrep/nmap.png?raw=true)

as we see on the screenshot above there is tree open ports.

- 22 default port for ssh
- 80 default port for http
- 33060 runs mysql server


### directory enumeration :

from the nmap scan, now we know that an http server is runing on this ip, we shoult run derectory enumeration to see if there is any directory or file that contain potetial sensitive date.
for this purpose i used a tool called dirsearch 

```bash
    python3 dirsearch.py -u <ip> -i 200
```
- -u for url
- -i to include only `200` response status

![directory inumeration image ](https://github.com/FR1NG/oscp/blob/master/screenshots/InfosecPrep/directory_enomuration.png?raw=true)

as we see on the screenshot above there is a file called /robots.txt

[robots.txt image]

on that file i found that there is another file called /secret.txt

[secret.txt image]

on that file i found that there is 64base encoded text.
after decoding it, i found out that it is an ssh private key, i decoded it a second time to get the name of the user for that key, a found out that the name of this user is oscp.

![private key image ](https://github.com/FR1NG/oscp/blob/master/screenshots/InfosecPrep/privatekey.png?raw=true)
![user image ](https://github.com/FR1NG/oscp/blob/master/screenshots/InfosecPrep/user.png)

i saved the private key on a file and i connect to over ssh using the key
```bash
    ssh -i private_key_file oscp@<ip>
```
- -i for identity file

![connected image](https://github.com/FR1NG/oscp/blob/master/screenshots/InfosecPrep/connected.png)

i ran linpeas script to detect if there is a way for me to gain root access, and a found out that i can run bash with root privileges.
after runing bash with `bash -p` now im root.
 ![root image](https://github.com/FR1NG/oscp/blob/master/screenshots/InfosecPrep/root.png?raw=true)
