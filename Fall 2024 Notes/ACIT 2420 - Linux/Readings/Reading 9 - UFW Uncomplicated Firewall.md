- Ufw stands for <mark style="background: #ABF7F7A6;">Uncomplicated Firewall</mark>, and is a program for managing a netfilter [firewall](https://wiki.archlinux.org/title/Firewall "Firewall"). 
- It provides a command line interface and aims to be uncomplicated and easy to use.
- It should be noted that UFW can use either [iptables](https://archlinux.org/packages/?name=iptables) or [nftables](https://archlinux.org/packages/?name=nftables) as the back-end firewall.

### Installation

[Install](https://wiki.archlinux.org/title/Install "Install") the [ufw](https://archlinux.org/packages/?name=ufw) package.

[Start](https://wiki.archlinux.org/title/Start "Start") and [enable](https://wiki.archlinux.org/title/Enable "Enable") `ufw.service` to make it available at boot. Note that this will not work if `iptables.service` is also enabled (and same for its ipv6 counterpart)

### Basic configuration

A very simplistic configuration which will deny all by default, allow any protocol from inside a 192.168.0.1-192.168.0.255 LAN, and allow incoming [Deluge](https://wiki.archlinux.org/title/Deluge "Deluge") and [rate limited](https://wiki.archlinux.org/title/Uncomplicated_Firewall#Rate_limiting_with_ufw) SSH traffic from _anywhere_:

```
# ufw default deny
# ufw allow from 192.168.0.0/24
# ufw allow Deluge
# ufw limit ssh
```

To allow a port instead from _anywhere_ use the following example to allow port 51312 UDP and TCP, port 51312 only UDP or a port range from 51312 to 51314:

```
# ufw allow 51312
# ufw allow 51312/udp
# ufw allow 51312:51314
```

The next line is only needed _once_ the first time you install the package:

`# ufw enable`

**Note:** Make sure `ufw.service` has been [enabled](https://wiki.archlinux.org/title/Enabled "Enabled").

Finally, query the rules being applied via the status command:

```
# ufw status

Status: active
To                         Action      From
--                         ------      ----
Anywhere                   ALLOW       192.168.0.0/24
Deluge                     ALLOW       Anywhere
SSH                        LIMIT       Anywhere
```

**Note:** The status report is limited to rules added by the user. For most cases this will be what is needed, but it is good to be aware that builtin-rules do exist. These include filters to allow UPNP, AVAHI and DHCP replies; for details consult the "Default ruleset" section in the [ufw README](https://git.launchpad.net/ufw/tree/README).

Extra information, including the default policies, can be seen with

`# ufw status verbose`

but this is still limited to user-specified rules. In order to see all rules setup

`# ufw show raw `

may be used, as well as further reports listed in the manpage. Since these reports also summarize traffic, they may be somewhat difficult to read. Another way to check for accepted traffic:

```
# iptables -S | grep ACCEPT
# ip6tables -S | grep ACCEPT
```

While this works just fine for reporting, keep in mind not to enable the `iptables` service as long as you use `ufw` for managing it.

**Note:** If special network variables are set on the system in `/etc/sysctl.d/*`, it may be necessary to update `/etc/ufw/sysctl.conf` accordingly since this configuration overrides the default settings.

### Forward policy

Users needing to run a [VPN](https://wiki.archlinux.org/title/VPN "VPN") such as [OpenVPN](https://wiki.archlinux.org/title/OpenVPN "OpenVPN") or [WireGuard](https://wiki.archlinux.org/title/WireGuard "WireGuard") can adjust the **DEFAULT_FORWARD_POLICY** variable in `/etc/default/ufw` from a value of **"DROP"** to **"ACCEPT"** to forward all packets regardless of the settings of the user interface. To forward for a specific interface like **wg0**, user can add the following line in the ***filter** block

/etc/ufw/before.rules

```
# End required lines 

-A ufw-before-forward -i wg0 -j ACCEPT
-A ufw-before-forward -o wg0 -j ACCEPT
```

You may also need to uncomment

/etc/ufw/sysctl.conf

```
net/ipv4/ip_forward=1
net/ipv6/conf/default/forwarding=1
net/ipv6/conf/all/forwarding=1
```

### Adding other applications

The PKG comes with some defaults based on the default ports of many common daemons and programs. Inspect the options by looking in the `/etc/ufw/applications.d` directory or by listing them in the program itself:

`# ufw app list`

If users are running any of the applications on a non-standard port, it is recommended to simply make `/etc/ufw/applications.d/custom` containing the needed data using the defaults as a guide.

**Warning:** If users modify any of the PKG provided rule sets, these will be overwritten the first time the ufw package is updated. This is why custom app definitions need to reside in a non-PKG file as recommended above!

Example, deluge with custom tcp ports that range from 20202-20205:

```
[Deluge-my]
title=Deluge
description=Deluge BitTorrent client
ports=20202:20205/tcp
```

Should you require to define both tcp and udp ports for the same application, simply separate them with a pipe as shown: this app opens tcp ports 10000-10002 and udp port 10003:

`ports=10000:10002/tcp|10003/udp`

One can also use a comma to define ports if a range is not desired. This example opens tcp ports 10000-10002 (inclusive) and udp ports 10003 and 10009

`ports=10000:10002/tcp|10003,10009/udp`

### Deleting applications

Drawing on the Deluge/Deluge-my example above, the following will remove the standard Deluge rules and replace them with the Deluge-my rules from the above example:

```
# ufw delete allow Deluge
# ufw allow Deluge-my
```

Query the result via the status command:

```
# ufw status

Status: active
To                         Action      From
--                         ------      ----
Anywhere                   ALLOW       192.168.0.0/24
SSH                        ALLOW       Anywhere
Deluge-my                  ALLOW       Anywhere
```

### Black listing IP addresses

It might be desirable to add ip addresses to a blacklist which is easily achieved simply by editing `/etc/ufw/before.rules` and inserting an iptables DROP line at the bottom of the file right above the "COMMIT" word.

/etc/ufw/before.rules

```
...
## blacklist section
# block just 199.115.117.99
-A ufw-before-input -s 199.115.117.99 -j DROP
# block 184.105.*.*
-A ufw-before-input -s 184.105.0.0/16 -j DROP

# don't delete the 'COMMIT' line or these rules won't be processed
COMMIT
```

### Rate limiting with ufw

ufw has the ability to deny connections from an IP address that has attempted to initiate 6 or more connections in the last 30 seconds. Users should consider using this option for services such as [SSH](https://wiki.archlinux.org/title/SSH "SSH").

Using the above basic configuration, to enable rate limiting we would simply replace the allow parameter with the limit parameter. The new rule will then replace the previous.

```
# ufw limit SSH

Rule updated

# ufw status

Status: active
To                         Action      From
--                         ------      ----
Anywhere                   ALLOW       192.168.0.0/24
SSH                        LIMIT       Anywhere
Deluge-my                  ALLOW       Anywhere
```

### User rules

All user rules are stored in `etc/ufw/user.rules` and `etc/ufw/user6.rules` for IPv4 and IPv6 respectively.