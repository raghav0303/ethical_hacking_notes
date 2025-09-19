
**Level 0** 
password found - ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

**Level 0 to 1** 
password found- ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If

**Level 1 to 2** 
password found - 263JGJPfgU6LtdEvgfWU1XP5yac29mFx

**Level 2 -> 3** 
password found - MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx

**Level 3 -> 4**
password found - 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

**Level 4 -> 5** 
password found - 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

**Level 5 -> 6** 
password found - HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
Command used
	$du -ab inhere | awk '{if($1 == 1033) {print $1, $2;}}'
	command that could have been used as well $find inhere -type f -size 1033c
	(c means bytes, f means type file only and size 1033 bytes that the file we are trying to find must contain)

	check for non executable files
	$find . -type f ! -executable
	
	to list non executable files with readable content
	$find . -type f ! -executable -exec file {} \; | grep -i "ascii text"
	
	single command for the question
	$find . -type f ! -executable -size 1033c -exec file {} \; | grep -i "ascii text"
	find cannot find human readable or ascii files as it finds files based on file metadata and cannot read the contents of the file.


Learnings
* ls -l doesnot give the actual size of the files, it gives logical size
* du(disk usage) or stat command can be used for finding actual size
* -a in du is for recursively seeing all the files inside a folder
* -b in du is for displaying the size of the files in bytes

**Level 6 -> 7**

inside root directory
command /$find . -size 33c -type f -user bandit7 -group bandit6 2>/dev/null

or(for not using cat command seperately)

/$find . -size 33c -type f -user bandit7 -group bandit6 2>/dev/null | xargs cat
(here we are required to use xargs, if we directly pass the path returned by find to cat then it will just echo the path in the output, for cat to understand that we are passing a path through pipe we use xargs)

This tells `cat` to read the file whose name was output by `find`.

or

/$ cat "$(find . -size 33c -type f -user bandit7 -group bandit6 2>/dev/null)"
(command substitution)
password found - morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

Learnings 
* we are using file descriptor to send the stderr output (denoted by 2) to black hole file /dev/null so that the Permission Denied messages donot come in output and also that the permission denied messages will not dissapear even if we grep a keyword in output since the stderr are errors and not output of find command

**Level 7 -> 8**

password found - dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

command $grep -i "millionth" data.txt

**Level 8 -> 9**

password found - 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

command $sort data.txt | uniq -u

Learning
* 'uniq'  does not detect repeated lines unless they are adjacent. You may want to sort the input first, or use 'sort -u'($sort -u data.txt -> displays all the unique lines once, eliminating repeated occurences of every line) without 'uniq'.

**Level 9 -> 10**

password found - FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

command $strings data.txt
or
$strings -e s data.txt | grep ==
the second one is more appropriate as it further extracts the relevant output only
Learning
* **The strings command** in Linux is a utility that extracts human-readable text strings from binary files. Binary files, such as executable programs and libraries, contain both machine code and text data.
* got grep to threat binary files as text files so that it can display matched outputs use -a or --text option

**Level 10 -> 11**
passwod - dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
command $base64 -d data.txt

to encode $base64 file_name.txt

**Level 11 -> 12**

password - 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4

command $cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'

Learning
* Here we are using tr command to substtute each letter by a letter 13 places after them as done in ROT13 cipher. This is working to decode ROT13 cipher because it is a reciprocal cipher so text encoded with ROT13 cipher can be decoded by the repeating the encoding process. As 13 + 13 = 26 that is the total number of alphabets.
* 'A-Za-z' represent the current letters. They will be replaced by 13 letters forward to them. So to do that we map letters appearing in range A-Z to N-ZA-M we have to read it in sequence as A replaced by N(13 letters forward to A comes N so A s replaced by N and so on) and further like that, we cannot directly write N-M range as it will not be sequential so we break the range in between. N-ZA-M. Similarly we do for lower case letters a-z replaced mapped with n-za-m.

**Level 12 -> 13**

temporary directory - tmp.FLhUd2oVyQ

password - FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn

reference link - https://mayadevbe.me/posts/overthewire/bandit/level13/
others - https://www.suse.com/c/making-sense-hexdump/

Learnings
* List of file signatures. File signatures can be seen at the starting hexadecimal values of a hexdump which tells us which type of file we have taken a hexdump of. So, hexdump of a file helps us to get information about the file we are seeing.

**Level 13 -> 14**

password - MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

commands 
first copying the sshkey.private to local system using scp

$scp -P 2220 bandit13@bandit.labs.overthewire.org:/home/bandit13/sshkey.private /home/raghav/Documents/shell_practice/overthewire_bandit/bandit13

loging in using rsa private key, first we need to change the permissions of the key file for only the user to have read permissions(over access should not be present for the private key file otherwise ssh doesnot accept it)
$chmod g-r sshkey.private

logging in as bandit14 using RSA private key
$ssh -i /home/raghav/Documents/shell_practice/overthewire_bandit/bandit13/
sshkey.private bandit14@bandit.labs.overthewire.org -p 2220

**Level 14 -> 15**

password - 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

command $nc bandit 30000
or $nc 127.0.0.1 30000
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS           ----------> value entered
Correct!                                                                    -------> output
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo           ----|

Learning
* localhost is our own system/system name(not user name).
* we can submit text or send text to a particular port using nc.

Reference 
* https://mayadevbe.me/posts/overthewire/bandit/level15/

**Level 15 -> 16**

password - kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

command$openssl s_client -connect bandit:30001
or 
$openssl s_client -connect localhost:30001
input on console - 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
output:
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

References
- https://mayadevbe.me/posts/overthewire/bandit/level16/
- https://www.feistyduck.com/library/openssl-cookbook/online/testing-with-openssl/connecting-to-tls-services.html

Level 16 -> 17

password :
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCScox+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

commands:
first we look at the ports that are open in the range 31000 to 32000
$nmap -PS -p31000-32000 bandit --reason --packet-trace --disable-arp-ping

we get 5 ports, now we can see the service and version running on these ports using -sV option. We first saw the open port and directly didnot use -sV with that scan because this -sV scan is a bit time taking scan
$nmap -sV -p 31046,31518,31691,31790,31960 bandit
output
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo

others were all echo except 1 for one port i.e. 31790. So we can connect it using openssl. we cannot use nc command to connect here with a port on which ssl/tls is running so we are using openssl. 
$openssl s_client -connect bandit:31790

something keyupdate comes when we enter password. to get rid of it we use -ign-eof
$openssl s_client -connect bandit:31790 -ign_eof -quiet

we used -quiet to get rid of other long output messages.

Reference 
* https://akashrajpurohit.com/blog/exploring-overthewire-level-16-to-level-17-bandit-challenge/

Learning
* we can see man page of a command inside command by writing both commands one after other, eg-$man openssl s_client ------> shows the man page for the s_client command of openssl command
* The `-ign_eof` flag ensures that the connection isn’t terminated prematurely.

**Level 17 -> 18**

password - x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

command $diff passwords.old passwords.new
 
< denotes lines present at the first file in command(here passwords.old)
\> denotes lines present at the second file in command(here psswords.new)

42c42 means that line 42 in the first file was changed to line 42 in the second file. Since passwords.new file has the password, we take the 42 line of passwords.new as the password for next level.

**Level 18 -> 19**

password - cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

commands:
$ssh -t bandit18@bandit.labs.overthewire.org -p 2220 "bash --noprofile --norc"
or 
$ssh bandit18@bandit.labs.overthewire.org -p 2220 /bin/bash   --------> the command promt appears without the path\$ sign after entering the password. So write ls to confirm.
or
$ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme ------------> directly executing the cat command remotely using ssh 

Reference
* https://mayadevbe.me/posts/overthewire/bandit/level19/

**Level 19 -> 20**

password - 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO

command $./bandit20-do cat /etc/bandit_pass/bandit20

Learnings:
* see references(important, we have entered priveledge escalation from this point)
* binary means an executable file. We can execute it by ./<name_of_binary> argument_if _any

references - 
* https://www.cbtnuggets.com/blog/technology/system-admin/linux-file-permissions-understanding-setuid-setgid-and-the-sticky-bit

**Level 20 -> 21**
password - EeoULMCra2q0dSkYj561DX7s1CpBuOBt

commands $nc -l 12345
$./suconnect 12345

giving the password as input in the terminal where 12345 port is listening(started listening using $nc -l 12345)

in the suconnect terminal the new password will be printed

Learning
* listening using nc command
* 


reference - https://mayadevbe.me/posts/overthewire/bandit/level21/

**Level 21 -> 22**

password - tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q

commands:

bandit21@bandit:~$ cd /etc/cron.d
bandit21@bandit:/etc/cron.d$ ls
clean_tmp         cronjob_bandit23  e2scrub_all  sysstat
cronjob_bandit22  cronjob_bandit24  otw-tmp-dir
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
\* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
bandit21@bandit:/etc/cron.d$ cd /usr/bin/cronjob_bandit22.sh
-bash: cd: /usr/bin/cronjob_bandit22.sh: Not a directory
bandit21@bandit:/etc/cron.d$ cd /usr/bin/
bandit21@bandit:/usr/bin$ ls

bandit21@bandit:/usr/bin$ ls | grep cron
cronjob_bandit22.sh
cronjob_bandit23.sh
cronjob_bandit24.sh
crontab
bandit21@bandit:/usr/bin$ cat cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit:/usr/bin$ cat /etc/bandit_pass/bandit22
cat: /etc/bandit_pass/bandit22: Permission denied
bandit21@bandit:/usr/bin$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q

References 
* https://mayadevbe.me/posts/overthewire/bandit/level22/
* https://en.wikipedia.org/wiki/Cron

Learnings

* reading how to see scheduled tasks using crontab
	* eg - @reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
			\* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null







