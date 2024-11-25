       _                     _ _ _   
      | |__   __ _ _ __   __| (_) |_ 
      | '_ \ / _` | '_ \ / _` | | __|
      | |_) | (_| | | | | (_| | | |_ 
      |_.__/ \__,_|_| |_|\__,_|_|\__|

[BANDIT](https://overthewire.org/wargames/bandit/)
### Level 0
Access the bandit game via ssh
```
ssh -p 2022 bandit0@bandit.labs.overthewire.org
```
### Level 0 -> Level 1
Get password for next game from file in home dir
```
cat readme
```
### Level 1 -> Level 2
Get password from file '-'
```
cat ./-
``` 
or 
```
cat < -
```
### Level 2 -> Level 3
Get password from file with spaces in its name
```
cat 'spaces in this filename'
``` 
or 
```
cat spaces\ in\ this\ filename
```
if you wanna be really verbose and hate using bash autocomplete
### Level 3 -> Level 4
Get password from hidden file in the 'inhere' directory
```
ls -lha inhere'
``` 
--> '...Hiding-From-You'
```
cat inhere/...Hiding-From-You
```
### Level 4 -> Level 5
Get password from the only human readable file in the 'inhere' directory
```
find inhere/ -type f -exec file {} \;
cat inhere/-file07
```
### Level 5 -> Level 6
Get password from file that is not executable, 1033 bytes in size and human-readable
```
find ~/inhere/ -type f -size 1033c -exec cat {} \;
```

### Level 6 -> Level 7
Get password from file somewhere on server that is owned by bandit7 user and by bandit6 group and is 33bytes in size.
```
id bandit6
id bandit7
``` 
to get id uid and gid for these users
```
find / -type f -gid 11006 -uid 11007 -size 33c 2>/dev/null -exec cat {} \;
```
### Level 7 -> Level 8
Get password from file data.txt next to the word 'millionth'.
```
grep 'millionth' data.txt
```
### Level 8 -> Level 9
Get password from data.txt. Only string to occur only once
```
sort data.txt | uniq -u 
```
### Level 9 --> Level 10 
Get password from data.txt. One of few human-readable strings prepended by '='
```
strings data.txt | grep "==="
```
### Level 10 -> Level 11 
Get password from data.txt encoded in base64
```
base64 -d data.txt
```
### Level 11 -> Level 12
Get password from data.txt that has been rot13'd
```
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
rot13 is a classic cs puzzle. it was smarter just to google its implementation in bash rather than waste time trawling through man pages
### Level 12 -> Level 13
Get password from data.txt, repeatedly compressed hexdump
```
TMPDIR=$(mktemp -d)
cd "$TMPDIR"
cp ~/data.txt .
xxd -r data.txt hexrev1.txt
```
get file into a writable directory
```
file hexrev1.txt
mv hexrev1.txt hexrev1.gz
gzip -dc > hexrev2
file hexrev2
mv hexrev2 hexrev2.bz2
bzip2 -dc hexrev2.bz2 > hexrev3
file hexrev3
mv hexrev3 hexrev3.gz
gzip -dc hexrev3.gz > hexrev4
file hexrev4
mv hexrev4 hexrev4.xz
tar xOf hexrev4.xz > hexrev5
file hexrev5
mv hexrev5 hexrev5.xz
tar xOf hexrev5.xz > hexrev6
file hexrev6
mv hexrev6 hexrev6.bz2
bzip2 -dc hexrev6.bz2 > hexrev7
file hexrev7
mv hexrev7 hexrev7.xz
tar xOf hexrev7.xz > hexrev8
file hexrev8
mv hexrev8 hexrev8.gzip
gzip -dc hexrev8.gzip > hexrev9
file hexrev9
cat hexrev9
```
A little laborious but simple in concept. Reverse the hexdump with `xxd -r` and then walk through a series of decompressions with `bzip2`, `gzip` and `tar` checking each time with `file` to find the right compression algorithm. I chose to use these decompressions writing to STDOUT just to keep track of each process and keep original files intact. Each command behaves slightly differently when it comes to overwriting file and determining decompressed filenames.
### Level 13 -> Level 14
Log in to next level with private ssh key
```
ssh -p 2220 -i sshkey.private bandit14@localhost
```
### Level 14 -> Level 15
Get password to next level by submitting current level password to port 30000
```
telnet localhost 30000
$: <current_level_password>
```
### Level 15 -> Level 16
Get password to next level by submitting current level password to port 30001 using ssl/tls encryption
```
ncat --ssl localhost 30001
$: <current_level_password>
```
### Level 16 -> Level 17    
Get password to next level by submitting current level password to port a port between 31000 and 32000. You must determine the correct port with ssl/tls.
```
nc -z localhost 31000-32000
```
or
```
nmap -p 31000-32000 localhost
```
these commands return a list of ports. Connect to each port with `ncat --ssl localhost <port>` and submit the current level password. One of them will return a correct answer. When the right one is found, the command may be re-run to write the return data to a file.
```
ncat --ssl localhost <correct_port> > sshkey

```
NOTE: this returns a private ssh key. In order for this to be functional, it must have its file permissinons altered. A simple `chmod 400 sshkey` will allow it to be used.
### Level 17 -> Level 18
Get password for next level from two files. The only line that has changed between these two files is the password.
```
diff passwords.old passwords.new
```
### Level 18 -> Level 19
Get password from file stored in a home directory without being able to spawn a shell.
```
ssh -p 2220 bandit18@bandit.labs.overthewire.org -t ls
ssh -p 2220 bandit18@bandit.labs.overthewire.org -t 'cat readme'
```

### Level 19 -> Level 20    
Use the binary in the home dir to execute a command as a different user to acquire the password in the usual place level passwords are stored.
```
./bandit20-do $(id bandit20) cat /etc/bandit_pass/bandit20
```
### Level 20 -> Level 21
Use the setuid binary to connect to a port that sends out the current password. If the binary receives the correct password it will respond with the password for the next level.

First we need a port number of our choosing.
```
PORT_NO=<chosen_port_no>
nc -l -p $PORT_NO < /etc/bandit_pass/$USER &
~/suconnect $PORT_NO
```
### Level 21 -> Level 22
Take a look in /etc/cron.d
```
cat /etc/cron.d/cronjob_bandit22
cat /usr/bin/cronjob_bandit22.sh    
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
### Level 22 -> Level 23
Take a look in /etc/cron.d
```
cat /etc/cron.d/cronjob_bandit23
cat /usr/bin/cronjob_bandit23.sh    
TMP=$(mktemp -d);cd $TMP
cp /usr/bin/cronjob_bandit23 .
vim cronjob_bandit23.sh
```
cronjob_bandit23.sh uses the current username in an md5 hashing function to generate a filename and then writes the current level password to /tmp/<hashed filename>. We just need to change the script so we can get the filename where the cronjob has put the password for the nex level
```
1 #!/bin/bash
2   
3 myname="bandit23"
4 mytarget="$(echo I am use $myname | md5sum | cut -d ' ' -f 1)
5 echo "$mytarget"
```
with the script changed to suit our needs we just need to run it to get the right filename and then cat the result
```
NEXT_PASS=$(bash ./cronjob_bandit23.sh)
cat /tmp/$NEXT_PASS
```
### Level 23 -> Level 24
Take a look in /etc/cron.d
```
cat /etc/cron.d/cronjob_bandit24
cat /usr/bin/cronjob_bandit24.sh
```
Catting the bandit24 cron job and its script shows that cron will run a job every minute as user bandit24 and will execute then delete any script placed in /var/spool/bandit24/foo. Current user bandit23 can write to this directory therefore we can use it for privelege escalation by writing our own script to broadcast the bandit24 password on a local port
```
TMP=$(mktemp -d);cd $TMP
touch next_pass.sh
chmod +x next_pass.sh
vim next_pass.sh
```
then edit the script to look somethiing like the following;
```
1 #! /bin/bash
2 nc -l -p <port_number> < /etc/bandit_pass/$(whoami) &
```
once the script is written you can copy it to the right directory where the cronjob_bandit24.sh executes, wait a minute for the cron job to run and then listen to the port to get the password for user bandit24
```
cp next_pass.sh /var/spool/bandit24/foo
sleep 60;nc localhost <port_number>
```
### Level 24 -> Level 25
Daemon on port 30002 has the password for the next level, but only if you submit the current password and a secret four digit code. You'll have to brute force it.
```
TMP=$(mktemp -d);cd $TMP
touch pw_brute-force.sh
chmod +x pw_brute-force.sh
vim pw_brute-force.sh
```
edit the script to look something like this then run it
```
1 #! /bin/bash
2 
3 if [ ! -f ./brute-force ]; then
4   touch ./brute-force
5   for i in {0000..9999}; do    
6       echo "<current_level_password> $i" >> ./brute-force
7   done
8 fi
9
10 cat ./brute-force | nc locahost 30002
```
### Level 25 -> Level 26
Log in to level 26. The shell for it is not /bin/bash. Figure it out

This one's kinda dumb. /home/bandit25/ has a ssh private key for bandit26. This time ssh via localhost is not allowed so you'll have to key the key locally
```
ssh -p 2220 bandit25@bandit.labs.overthewire.org -t 'cat ~/bandit26.sshkey' > bandit26.key
```
Having this key, its easy then to ssh into bandit26
```
ssh -p 2220 -i bandit26.key bandit26@bandit.labs.overthewire.org
```
Problem is, the shell exits after outputting a small bit of text on a successful connection. I won't write out the solution here, I had to google it and I don't want to pass it off that I figured it out. It involves making your terminal window only a few lines in height when you ssh into bandit26 and from there you can get a shell through vi. As someone who's termninal is ALWAYS fullscreen this one sucked.
### Level 26 -> Level 27
Now that you have a shell get the pass for level 27

Following on from the last level, you should still have a shell - at least an instance of vi. If you don't know vim this might be a pain, but otherwise it's a breeze.
```
:! ls
:! bandit27-do cat /etc/bandit_pass/bandit27
```
### Level 27 -> Level 28
Clone a repository and find a password
```
TMP=$(mktemp -d);cd $TMP
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
cat repo/README
```
