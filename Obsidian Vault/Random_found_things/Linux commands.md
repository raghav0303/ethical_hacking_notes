- This command removes the VirtualBox application and purges its configuration files.

	`sudo apt --purge remove virtualbox*`

	**`*` (wildcard)**: The asterisk wildcard matches any characters that follow. This tells `apt` to find and remove any installed package whose name begins with "virtualbox". This is useful because VirtualBox might have several related packages, such as `virtualbox-dkms` or `virtualbox-guest-additions`, and this ensures all of them are removed at once.
	
	This command removes any packages that were installed as dependencies of VirtualBox but are no longer needed by any other installed software.
	
	`sudo apt autoremove && sudo apt autoclean`

	command that may be needed after this
	    - `sudo delgroup vboxusers`
	    - ```rm -rf ~/.config/VirtualBox/    
		rm -rf ~/VirtualBox\ VMs/    
		rm -rf ~/.VirtualBox/```

- Virtual box not able to start any vm because kvm is running in the background
	fixes:
		`lsmod | grep kvm`     (displays status of modules related to kvm)
		`sudo lsof /dev/kvm`   (lists open files related to kvm)
		`sudo kill -9 <pid> <pid>` (kills the kvm processes whose pid are mentioned in the command one after other)
		