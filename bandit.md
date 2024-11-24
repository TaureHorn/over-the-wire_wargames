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
`cat 'spaces in this filename` or `cat spaces\ in\ this\ filename` if you wanna be really verbose and hate using bash autocomplete
## Level 3 -> Level 4
Get password from hidden file in the 'inhere' directory
`ls -lha inhere'` --> '...Hiding-From-You'
`cat inhere/...Hiding-From-You`

