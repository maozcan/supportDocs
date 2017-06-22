# Various commands that I mostly use

| Command | Function |
|-------:| -------|
| `sudo raspi-config` | Initial configuration after installing the image
|`sudo apt-get update`| Update packages
|`sudo apt-get upgrade`| Upgrade packages
|`sudo apt-get dist-upgrade`| Upgrade distribution
|`sudo rpi-update`| Get the newest kernel and firmware
|`apt-get install ntpdate`
|`ntpdate uk.pool.ntp.org`| rpi-update will only work after time has been set
|`apt-cache search xxxxx`| Search for packages
|`dpkg --get-selections`| List installed packages
|`dpkg -L name`| List files in Installed Package
|`dpkg -c filename.deb`| List files in Installed Package
|`dpkg-query -S filename`| Check if package owns file
|`apt-cache show xxxx`| Show package information
|`sudo apt-get install xxxx `| Install package.. Add -y after apt-get to confirm installation of packages. multiple packages may be installed at once
|`sudo apt-get remove xxxx`| Remove package
|`passwd`| Change the current user's password
|`sudo passwd root`| Change root user password
|`sudo sync`|
|`sudo reboot`| Reboot system
|`sudo halt`| Shutdown system
|`wget http://xxx.com/yyy.tar.gz`| Download a file to current directory
|`tar xvzf yyyy.tar.gz`| Untar yyyy.tar.gz to current directory
|`ps ax ... grep avahi`| Check if Avahi service is installed/running
|`sudo service avahi-daemon status`| Check the status of avahi daemon
|`gpio -v`| Show gpio verison
|`gpio readall`| Show gpio ports and status
|`arp`| Scan network devices
|`sudo rm /etc/ssh/ssh_host_* && sudo dpkg-reconfigure` |  
|`openssh-server`| Regenerate RSA/DSA kes for OpenSSH
|`apt-get install ntp fake-hwclock`| Set time
|`dpkg-reconfigure tzdata`| Choose timezone
|`apt-get install locales && dpkg-reconfigure locales`| Configure locale settings
|`apt-get install console-setup keyboard-configuration && dpkg-reconfigure keyboard-configuration`| Configure keyboard settings
|`sudo -i`| Become root user
|`tail -f /path/to/filr -n 5`| View last 5 lines of the file
|`tail -f /var/log/syslog`| Watch changes to a file
|`sudo /etc/init.d/servicename restart`| Stop/Start a service
|`ifconfig`| displays current network config
|`sudo ifdown eth0/eth1/wlan0/wlan1 `| bring down network interface
|`sudo ifup eth0/eth1/wlan0/wlan1`| bring up network interface
|`head -10 filename `| prints the first 10 lines in a file
|`tail -20 filename`| prints the last 20 lines in a file
|`less filename`| better more, lists the contents of a file - paginated. If you want to continuously parse a log but only search for keywords:
|tail -f logfile &#124; grep string| this prints all lines that contains string
|tail -f logfile &#124; grep -v string| this prints all lines that doesn't contain string
|**Searching**|
|`find -name "*.txt"`| show all files with .txt extension
|`find /home/foo -name "*.gz" `| search in specific path
|`find -newer bar.txt`| find all files newer than bar.txt
|`df -h`| Get information about the disks connected
|`mount`| Get information the devices mounted
|`dmesg`| show log of mounts
|`lsusb -t`| Explore USB devices connected to the Pi
|`fdisk -l`| Show disk partitions
|`nmap -T4 -F -O 192.168.1.1`| Scan open ports on a device
|`find . -name *.log -print`| Find all files with log extension
|`grep -Ril "text-to-find" .`| Find files containing text-to-find in current and sub directories
|`sudo apt-get install xrdp`| Install Remote Desktop client
|`sudo nano /etc/xrdp/xrdp.ini`| Edit options for the Remote Desktop (xrdp) server
