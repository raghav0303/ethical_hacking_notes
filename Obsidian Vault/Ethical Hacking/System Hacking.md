- Identifying services running on the system using nmap
- services like ftp, telnet, ssh and login enable us to gain controle over the target system
- Ways
	-Using telnet
	- command: telnet <ip of target system>
	- Using ssh
		- command: ssh <ip address of target> -l <username of system>
			or
		- command: ssh <username of target>@<ip address of target>
	-> when there are no known credentials
		- use tools like crunch to perform dictionary attack
			- eg:
				$crunch 1 2
				$crunch 1 2 0123456789
				$crunch 4 4 -t ,@%^   (,-all upper case letters, @-all lower case letters, %-all numbers, ^-all special symbols)
				$crunch 4 4 -p abcd
				$crunch 4 4 abcd > pass.txt
		- In general hydra tool is used to perform password attack using large dictonaries
			- eg:
				$hydra -L pass.txt -P pass.txt 192.168.1.8 ssh -t 4
			- Graphical user interface of hydra tool in Xhydra tool
		-  Other password cracking tools are john the ripper, rtgen, hasket etc
	-> User enumeration(collecting the details of users that can access the system)
		- priviledge escalation
			- done using nmap scriptt smb enum users or tools like enum for linux and rpc client
			- smb serviceused for user enumeration
				- eg: nmap --script smb-enum-users 192.168.1.8
				- in windows
					- local administrator - rid 500s
					- standard user - rid 1000s
					- guest user - rid 501s
					- >whoami /all
							(command to give all user information in windows)
							last 4 digits of sid is the rid of the user
			- enum4linux tool usage
				-$enum4linux -U 192.168.1.8
			- rpcclient tool also used(given in system hacking video of ethical hacking nptel course)
	-> for file transfer between target and attacker system we can use ftp service. we need to start the ftp session while in the directory which contains the files to be transfered in kali linux
		- command: ftp <ip address of target>
			- enter username and password
			- on successful login we can now use the ftp terminal
			- to transfer a file to target machine
				- eg: put pass.txt
			- to download a file from target
				- eg: get pass.txt
			- to delete a file
				- eg: delete pass.txt
	-> to execute applications in the taget system
		- we can use remote login services like ssh or ternet but not ftp as ftp doesnot recognise commands like sudo, apt-get etc
		- for file upload and application execution we can also use backdoors and malwares