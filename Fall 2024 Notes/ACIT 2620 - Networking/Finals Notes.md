### Making a new router
- Linked clone to centos 8
- make new MAC address
- allow all promiscuous mode 
- edit the ip addresses in the nmtui
- need to enable ipv4 forwarding for new routers 
	- `sudo vim /etc/sysctl.conf`
	- add in `net.ipv4.ip_forward = 1`
	- then do `sudo sysctl --system` to reload after change

### DHCP 
- edit dhcp files in `/etc/dhcp/dhcpd.conf`

R1 dhcpd.conf example
```
option domain-name "2620.acit"
oprion domain-name-servers 8.8.8.8, 10.20.30.254;

subnet 172.16.1.0 netmask 255.255.255.224 {
	option routers 172.16.1.1;
	range 172.16.1.2 172.16.1.31;
}

host ws1 {
	hardware ethernet 02:00:00:00:00:04;
	fixed-address 172.16.1.30;
}
```
- `sudo dhcpd -t` to check syntax errors
- `sudo systemctl start dhcpd.service`
- `sudo systemctl enable dhcpd.service`

**if making changes:** 
- `sudo systemctl daemon-reload`
- `sudo systemctl restart dhcp`
#### Troubleshooting
- `systemctl status dhcpd.service`
- `sudo systemctl restart dhcpd`

### OSPF/Bird
- OSPF (bird) file in `/etc/bird.conf`

R1 bird.conf example
```
router id 10.20.30.100;

protocol device {

}

protocol kernel {
	ipv4 {
		export all;
	};
}

protocol ospf {
	area 0 {
		interface "enp0s3" {
		};
		
		interface "br0" {
			stub;
		};
	};
}
```
- `sudo bird -p`
- `systemctl start bird.service`
- `sudo systemctl enable bird.service`

**If making changes:** 
- `sudo systemctl daemon-reload`
- `sudo systemctl restart bird`
- `sudo systemctl status bird`
#### Troubleshooting
- `systemctl status bird.service`
- `sudo birdc show ospf interface`