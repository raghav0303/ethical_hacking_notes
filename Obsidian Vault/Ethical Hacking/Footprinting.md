Passive Reconnaisance
	-> archive.org, whois, netcraft, harvester
	-> search engines and search operators
	-> dig tool

Active Reconnaissance
	-> Nmap, nessus, Zenmap, nexposemetasploit framework
	-> mail enumeration, dns enumeration, email enumeration
		- DNS server generally port 53
		- host tool, website name to ip address, dns lookup
		- dig tool - for dns enumeration
		- dnsenum - for dns enumeration , less query options needed to be given for good results
		- dnsrecon - automated dns enumeration
	-> scanning
		- nmap - vulnerability scanning and network discovery
			- Host discovery (detecting live or running hosts)
				- ICMP sweep(-PE option)
					- eg# nmap -PE -sn 192.168.1.3 --reason --packet-trace --disable-arp-ping
					- in windows ICMP sweep scan doesnt provide correct data if firewall is on
				- Broadcast ICMP 
				- Non-echo ICMP (-PP and -PM)
					- Timestamp (-PP)
					- Address mask request(-PM) - not able to detect running host even if firewall of windows is off and failed for metasploit 2 as well
				- TCP Sweep
					- TCP SYN(-PS)
					- TCP ACK(-PA)
				- UDP sweep
					- uses -PU option
					- unreliable
			- Port Scanning
				- here -sn option is not used, besides that it is same as host discovery
				- TCP SYN scan(-sT option used)
					- full connection is established between attacker and target system so risky
					- fast and supports IPv6 version
					- doesnot require root previlidges
				- TCP Stealth scan(-sS)
					- default scanning option of nmap
					- inverse mapping, flag probe packets
					- eg: nmap -sS -p22 192.168.1.3 --reason --packet-trace --disable-arp-ping
						(here we are mentioning port 22 to be scanned in particular)
				- FTP bounce scan (-b)
					- for data transfer with an anonymous ftp server of host
					- if we donot provide username and password in the command then by default the username will be anonymous and password will be wwwuser@
					- use -v(verbose option) with -b for more detailed results
					- eg: 
						- nmap -b <ip of target system> <ip of client system or kali linux that is attacking>
						- nmap -v -b <username>:<password>@<ip of target system> <ip of client system or kali linux which is attacking>
							- nmap -v -b user:user@192.168.1.25 192.168.1.28
							- nmap -p22 -Pn -v -b 192.168.110.27 192.168.110.48
											or
							- nmap -Pn -v -b user:user@192.168.110.27:22 192.168.110.48
			- Detecting OS and version detecion of servics running on the target (-O and -sV or -A)
				- nmap uses all types of sweeps(except UDP sweep) for host detection(options are -sP, -sn, -sl, -Pn)
				- linux os replies with sequence id 0 and windows replies with sequence previous received packet id + 1
			- Vulnerability scanning
				- nmap scripts
					- nmap --script <script name> <ip address of the target system>
					- https://nmap.org/book/man-nse.html
					- ls -al /usr/share/nmap/scripts 
						(to get list of all the scripts)

Proxy Setup
	- hide.me extention to change public ip address
	- proxychain tools
	- macchanger tool