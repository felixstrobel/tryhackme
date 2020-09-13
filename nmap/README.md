20.08.2020 | Felix Strobel ([relevant](https://tryhackme.com/p/relevant))

# Nmap

  - [Task 1](#task-1)
  - [Task 2](#task-2)
  - [Task 3](#task-3)

## Task 1

<i>[Nmap](https://nmap.org) is an incredibly valuable tool in the world of penetration testing. In this room, we will cover the basics from using nmap to effectively scan a target, gaining insight for further attacks.</i>

1. Deploy the machine.<br>
   `No answer needed.`

## Task 2

<i>To open the help menu of nmap we can use the `-h` flag.<br>
The full command looks like this: `nmap -h`</i>

1. How do you access the help menu?<br>
   `-h`

<i>The answers for the next twelve questions can be found on the help menu of nmap or can be concluded.</i>

2. What is the first switch listed for a `Syn Scan`?<br>
   `-sS`

3. What is the first switch listed for a `UDP Scan`?<br>
   `-sU`

4. How to detect operating systems?<br>
   `-O`

5. How about service version detection?<br>
   `-sV`

6. Most people like to see some output to know that their scan is actually doing things, what is the verbosity flag?<br>
   `-v`

7. What about 'very verbose'?<br>
   `-vv`

8. Sometimes saving output in a common document format can be really handy for reporting, how do we save output in xml format?<br>
   `-oX`

9. Aggressive scans can be nice when other scans just aren't getting the output that you want and you really don't care how 'loud' you are, what is the switch for enabling this?<br>
    `-A`

10. How do I set the timing to the max level, sometimes called 'Insane'?<br>
    `-T5`

11. What about if I want to scan a specific port?<br>
    `-p`

12. How about if I want to scan every port?<br>
    `-p-`

13. What if I want to enable using a script from the nmap scripting engine?<br>
    `--script`

<i>To list all possible categories we have to visit nmap's [homepage](https://nmap.org/book/nse-usage.html). Now we can see that `vuln` is a possibility.</i>

14. What if I want to run all scripts out of the vulnerability category?<br>
    `--script vuln`

<i>This switch can also be found on the help menu.</i>

15. What switch should I include if I don't want to ping the host?<br>
    `-Pn`

## Task 3

<i>We can perform a syn scan on the target machine. To do so we can use the `-sS` flag. It could be that you need root privileges to run this command. In this case, run the command with `sudo`.<br>
The full command looks like this: `nmap -sS MACHINE_IP`</i>

1. What will this command be without the MACHINE_IP address?<br>
   `nmap -sS`

<i>After scanning, nmap returns the following output. We can see that two ports are exposed by the machine.</i>

<pre>
PORT   STATE SERVICE
<b>22/tcp open  ssh</b>
<b>80/tcp open  http</b>
</pre>

1. How many ports do we find open under 1000?<br>
   `2`

<i>We can also find the protocol given for the ports. `tcp` stands for `Transmission Control Protocol`. It is part of some common network protocols.</i>

3. What communication protocol is given for these ports following the port number?<br>
   `tcp`

<i>The next question asks for the ssh service version. To scan for versions we can use the `-sV` flag.<br>
The full command looks like this: `nmap -sV MACHINE_IP`<br>
Now we also see the versions of the services.</i>

<pre>
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH <b>6.6.1p1</b> Ubuntu 2ubuntu2.10 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
</pre>

4. What is the version of the software running on port 22?<br>
   `6.6.1p1`

<i>Now we can perform an aggressive scan with the `-A` flag.<br>
The full command looks like this: `nmap -A MACHINE_IP`<br>
After the command has finished we can see which flag is not set.</i>

<pre>
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 58:48:ca:17:66:5d:80:74:a0:a1:b0:fd:3a:cd:3a:d7 (DSA)
|   2048 04:8c:1c:47:43:9d:e2:3a:51:bd:64:72:3a:da:3a:25 (RSA)
|   256 a8:48:35:40:90:dc:48:41:bd:46:32:aa:31:c2:40:28 (ECDSA)
|_  256 23:34:11:67:a4:d7:51:af:c7:4b:59:c0:2a:a1:3f:e1 (ED25519)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      <b>httponly</b> flag not set
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Apache/2.4.7 (Ubuntu)
| http-title: Login :: Damn Vulnerable Web Application (DVWA) v1.10 *Develop...
|_Requested resource was login.php
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
</pre>

5. What flag is not set under the results for port 80?<br>
   `httponly`

<i>At this point we can scan for some vulnerabilities. Using the `--script vuln` flag enables us to search for vulnerabilities.</i>

<pre>
PORT   STATE SERVICE
22/tcp open  ssh
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
80/tcp open  http
|_clamav-exec: ERROR: Script execution failed (use -d to debug)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|       httponly flag not set
|   /login.php: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-enum: 
|   /login.php: Possible admin folder
|   /robots.txt: Robots file
|   /config/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)'
|   /docs/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)'
|_  /external/: Potentially interesting directory w/ listing on 'apache/2.4.7 (ubuntu)'
| <b>http-slowloris-check</b>: 
|   VULNERABLE:
|   Slowloris DOS attack
|     State: LIKELY VULNERABLE
|     IDs:  CVE:CVE-2007-6750
|       Slowloris tries to keep many connections to the target web server open and hold
|       them open as long as possible.  It accomplishes this by opening connections to
|       the target web server and sending a partial request. By doing so, it starves
|       the http server's resources causing Denial Of Service.
|       
|     Disclosure date: 2009-09-17
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2007-6750
|_      http://ha.ckers.org/slowloris/
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
</pre>

6. What denial of service (DOS) attack is this box susceptible to?<br>
   `http-slowloris-check`
