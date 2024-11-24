# [BANDIT](https://overthewire.org/wargames/bandit/)
## Level 0
Access the bandit game via ssh
`ssh -p 2022 bandit0@bandit.labs.overthewire.org`
## Level 0 -> Level 1
Get password for next game from file in home dir
`cat readme`
## Level 1 -> Level 2
Get password from file '-'
`cat ./-` or `cat < -`
## Level 2 -> Level 3
Get password from file with spaces in its name
`cat 'spaces in this filename'` or `cat spaces\ in\ this\ filename` if you wanna be really verbose and hate using bash autocomplete
## Level 3 -> Level 4
Get password from hidden file in the 'inhere' directory
`ls -lha inhere'` --> '...Hiding-From-You'
`cat inhere/...Hiding-From-You`
## Level 4 -> Level 5
Get password from the only human readable file in the 'inhere' directory
```
find inhere/ -type f -exec file {} \;
cat inhere/-file07
```
## Level 5 -> Level 6
Get password from file that is not executable, 1033 bytes in size and human-readable
`find ~/inhere/ -type f -size 1033c -exec cat {} \;`

## Level 6 -> Level 7
Get password from file somewhere on server that is owned by bandit7 user and by bandit6 group and is 33bytes in size.
```
id bandit6
id bandit7
``` 
to get id uid and gid for these users
```
find / -type f -gid 11006 -uid 11007 -size 33c 2>/dev/null -exec cat {} \;
```
