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
$: $current_level_password
```
### Level 15 -> Level 16
Get password to next level by submitting current level password to port 30001 using ssl/tls encryption
```
ncat --ssl localhost 30001
$: $current_level_password
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
these commands return a list of ports. Connect to each port with `ncat --ssl localhost $port` and submit the current level password. One of them will return a correct answer. When the right one is found, the command may be re-run to write the return data to a file.
```
ncat --ssl localhost $correct_port > sshkey

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





