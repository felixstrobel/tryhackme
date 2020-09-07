21.09.2020 | Felix Strobel ([relevant](https://tryhackme.com/p/relevant))

# Crack The Hash

  - [Task 1](#task-1)
  - [Task 2](#task-2)

## Task 1

<i>The first hash looks like an `MD5` hash. Let's use [hashcat](https://hashcat.net/hashcat) to crack it. By using the `-m` flag we can specify the hash-type. With `hashcat -h` we can see a list of all possible modes. We can also see that `MD5` refers to mode 0. As our wordlist we will use the [`rockyou.txt`](https://gitlab.com/kalilinux/packages/wordlists/-/blob/kali/master/rockyou.txt.gz).<br>
The full command  looks like this: `hashcat -m 0 48bb6e862e54f2a795ffc4e541caed4d ./rockyou.txt`<br>
In a lot of output we can detect the cracked password.</i>

<pre>
48bb6e862e54f2a795ffc4e541caed4d:<b>easy</b>
</pre>

1. 48bb6e862e54f2a795ffc4e541caed4d<br>
   `easy`

<i>The next hash is produced from a `SHA1` hash algorithm. The command is the same but we will change the mode to 100 for `SHA1`.<br>
The full command: `hashcat -m 100 CBFDAC6008F9CAB4083784CBD1874F76618D2A97 ./rockyou.txt`<br>
And again it got cracked and we see the password.</i>

<pre>
cbfdac6008f9cab4083784cbd1874f76618d2a97:<b>password123</b>
</pre>

2. CBFDAC6008F9CAB4083784CBD1874F76618D2A97<br>
   `password123`

<i>This hash is also a `SHA` one but this time not a `SHA1`. It is a `SHA2-256` with the mode code 1400.<br>
Here is the command: `hashcat -m 1400 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032 ./rockyou.txt`<br>
This one was also easy to crack.</i>

<pre>
1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032:<b>letmein</b>
</pre>

3. 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032<br>
   `letmein`

<i>Here we have a `bcrypt-blowfish` hash. It has the mode number 3200. For this hash it is easier if we `echo` the hash into a file. Do it like this: `echo \$2y\$12\$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom > hash`. We have to escape the `$`.<br>
The full command looks like this: `hashcat -m 3200 hash ./rockyou.txt`<br>
This hash can take a while to crack. Be pacient and you will get the cracked hash.</i>

<pre>
$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom:<b>bleh</b>
</pre>

4. $2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom<br>
   `bleh`

<i>This hash is an `MD4` hash and has the mode number 900.<br>
The full command looks liek this: `hashcat -m 900 279412f945939ba78ce0758d3fd83daa ./rockyou.txt`</i>

<pre>
279412f945939ba78ce0758d3fd83daa:<b>Eternity22</b>
</pre>

5. 279412f945939ba78ce0758d3fd83daa<br>
   `Eternity22`

## Task 2

<i>Now we are reaching the more difficult hashes. The first one is a `SHA256` and has the number 1400.<br>
The full command looks like this: `hashcat -m 1400 F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85 ./rockyou.txt`<br>
Hashcat almost instantly cracks the hash.</i>

<pre>
f09edcb1fcefc6dfb23dc3505a882655ff77375ed8aa2d1c13f640fccc2d0c85:<b>paule</b>
</pre>

1. F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85<br>
   `paule`

<i>The second hash is a `NTLM` hash. NTLM is an authentication procedure for computer networks. It has the number 1000.<br>
So the full command looks like this: `hashcat -m 1000 1DFECA0C002AE40B8619ECF94819CC1B ./rockyou.txt`</i>

<pre>
1dfeca0c002ae40b8619ecf94819cc1b:<b>n63umy8lkf4i</b>
</pre>

2. 1DFECA0C002AE40B8619ECF94819CC1B<br>
   `n63umy8lkf4i`

<i>The third question gives us a really long hash. It is `SHA512` with the number 1800 and for this one we again have to `echo` the hash in a file. To do so we can use: `echo \$6\$aReallyHardSalt\$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02. > hash`. Now we can start cracking.<br>
The full command looks like this: `hashcat -m 1800 hash ./rockyou.txt`</i>

<pre>
$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.:<b>waka99</b>
</pre>

3. $6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.
   `waka99`

<i>The last hash is in a `HMAC-SHA1` format and has the number 160.<br>
The full command looks like this: `hashcat -m 160 e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme ./rockyou.txt`</i>

<pre>
e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme:<b>481616481616</b>
</pre>

4. e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme<br>
   `481616481616`
