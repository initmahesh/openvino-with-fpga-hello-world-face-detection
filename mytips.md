if you get issue during build as Temporary failure resolving blah 

then follow belwo steps to resolve
SOLUTION:
	On the host (I'm using Ubuntu 16.04), find out the primary and secondary DNS server addresses:
$ nmcli dev show | grep 'IP4.DNS'
IP4.DNS[1]:              10.0.0.2
IP4.DNS[2]:              10.0.0.3
Using these addresses, create a file /etc/docker/daemon.json:
$ sudo su root
# cd /etc/docker
# touch daemon.json
Put this in /etc/docker/daemon.json:
{                                                                          
    "dns": ["10.0.0.2", "10.0.0.3"]                                                                           
}     
Exit from root:
# exit
Now restart docker:
$ sudo service docker restart
VERIFICATION:
Now check that adding the /etc/docker/daemon.json file allows you to resolve 'google.com' into an IP address:
$ docker run --rm busybox nslookup google.com
Server:    10.0.0.2
Address 1: 10.0.0.2
Name:      google.com
Address 1: 2a00:1450:4009:811::200e lhr26s02-in-x200e.1e100.net
Address 2: 216.58.198.174 lhr25s10-in-f14.1e100.net
REFERENCES:
I based my solution on an article by Robin Winslow, who deserves all of the credit for the solution. Thanks, Robin!
"Fix Docker's networking DNS config." Robin Winslow. Retrieved 2016-11-09. https://robinwinslow.uk/2016/06/23/fix-docker-networking-dns/
