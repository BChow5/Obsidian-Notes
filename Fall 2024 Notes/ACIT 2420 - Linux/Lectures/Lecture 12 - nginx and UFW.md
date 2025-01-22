# nginx

## [](#what-is-a-web-server)What is a web server

In general the term server is used interchangeably to describe several things. For convenience we are going to group these into two categories:

- A hardware server
- A web server

Although technically the solution we have been using this term with digitalocean is a VPS (virtual private server). We could think of this as being functionally similar to a hardware server. This is a remote machine, running an OS purpose built for running on a server.

A web server is software that we can run on our hardware server. The purpose of which is serve documents over the internet. Generally using HTTP(s). Web servers help to provide secure access to documents. Many provide additional features like load balancing and proxy forwarding(more on this next week)

**Resources:**

[DO, Introduction to Web Servers](https://www.digitalocean.com/community/conceptual-articles/introduction-to-web-servers)

## [](#intro-to-nginx)intro to nginx

If you search for nginx (engine x) you will find a .com and .org. The .com is a SaaS (software as a service). The .org is the open source web server. This is the one we are looking at today.

nginx is a popular web server created by Igor Sysoev in 2005.

**Resources:**

[nginx.org](https://nginx.org/en/)

## [](#installing-nginx-on-arch-linux)installing nginx on Arch Linux

```
sudo pacman -S nginx
```

> [!question] The nginx service isn't running when we first install it. How do you start the service?
> - sudo systemctl start nginx or sudo systemctl enable nginx (enable start on boot)

## [](#configuring-nginx)configuring nginx

> [!Question] Where is the main nginx configuration file?
> - `/etc/nginx/nginx.conf`

The Arch Wiki document in last weeks flipped learning material has a good configuration example to get you started.

There are two configuration components that we are interested in today. The main nginx configuration in `/etc/nginx/nginx.conf` and server block configuration. The arch wiki document points out that there are two methods for creating server blocks. I personally don't have an opinion on which method is better, and there is not currently a single industry standard. Furthermore, there should be no difference in performance. So I am going to let you decide on which method you use for the assignment.

A server block allows you to setup multiple web servers on a single server(Linux server).

```
server {
    listen 80;
    listen [::]:80;
    
    server_name domainname1.tld;
    
    root /usr/share/nginx/domainname1.tld/html;
    index index.html;

	location / {
        try_files $uri $uri/ =404;
    }
}
```

- listen: which port to listen on, we are going to use 80(http) this week
    - 443 is https(in production you would use https)
    - `listen [::]:80;` specifies ipv6
- server_name: is the domain name or ip address of incoming traffic.
    - Since we don't have a domain name this could be our droplets ip address
    - You can also use the wildcard `_` which just specifies any server_name
    - You can't have multiple wildcard server blocks.
- root: This is directory that contains the content(HTML, CSS... files) that you want to serve
- index: This specifies the name of the index, or root document.
    - This can also go in location
- location /: This specifies what should be done when a user requests our project root, ie if they go to `website.com` instead of `website.com/about`.
    - This allows you to configure your server to perform different actions when a user visits a different path. More on this next week, for this week we are only going to serve /
    - **`try_files $uri $uri/ =404;`**:
        - **`try_files`**: This directive is used to check for the existence of specific files or directories in a specified order and serve them if they exist. If none of the files are found, Nginx will fall back to a default behavior (in this case, returning a 404 error).
        - **`$uri`**: This is a variable that represents the requested URI (the path of the request). For example, if a user visits `http://example.com/about`, the `$uri` would be `/about`.
        - **`$uri/`**: This represents the same requested URI but with a trailing slash appended. This is useful for checking whether the request corresponds to a directory. For example, if the user requests `/about` and it's a directory, Nginx will look for `/about/`.
        - **`=404`**: If none of the previous conditions (i.e., `$uri` or `$uri/`) are satisfied (i.e., the requested file or directory does not exist), Nginx will return a `404 Not Found` error. The `=` symbol makes the error code `404` a hard fallback, meaning the error is returned immediately without trying any other rules.

## [](#testing-nginx-configuration)testing nginx configuration

After editing nginx configuration you can validate it with the command:

```
sudo nginx -t
```

This will test `/etc/nginx/nginx.conf` and files referenced by the nginx.conf file.

**Resources:**

- [Nginx Docs](https://nginx.org/en/docs/)
    - [https://docs.nginx.com/search.html#sort=relevancy](https://docs.nginx.com/search.html#sort=relevancy)
    - [Alphabetical index of directives](http://nginx.org/en/docs/dirindex.html)
- [Arch Wiki nginx](https://wiki.archlinux.org/title/Nginx)

# [](#ufw)ufw

> [!Question] What is a firewal?

## [](#why-use-a-firewall)why use a firewall?

> The primary use case for a firewall is security. Firewalls can intercept incoming malicious traffic before it reaches the network, as well as prevent sensitive information from leaving the network.
> 
> Firewalls can also be used for content filtering. For example, a school can configure a firewall to prevent users on their network from accessing adult material. Similarly, in some nations the government runs a firewall that can prevent people inside that nation-state from accessing certain parts of the Internet.
> 
> - [Cloudflare What is a firewall?](https://www.cloudflare.com/learning/security/what-is-a-firewall/)

## [](#ufw-uncomplicated-firewall)UFW (Uncomplicated Firewall)

UFW is a program that makes it a little easier to manage a [netfilter](https://netfilter.org/) firewall. UFW actually manages nftables or iptables, which make use of netfilter which is part of the Linux kernel.

## [](#install-and-enable-the-ufw-package)install and enable the `ufw` package

`ufw` is available in the Arch package repository, so you can install it with `pacman`. Don't forget to update first if you haven't updated your server recently.

```
sudo pacman -S ufw
```

There are two parts to enabling `ufw`

- The ufw service managed by systemd
- The configured firewall, managed with the `ufw` utility itself

> [!warning] don't enable your firewall until you have allowed an ssh connection

Enable and start the service with `systemctl`

```
sudo systemctl enable --now ufw.service
```

## [](#configuration-files)configuration files

System wide configuration files for ufw can be found in `/etc/ufw`. Generally most of the configuration of ufw is done with `ufw` commands.

## [](#setting-up-a-firewall-with-ufw)setting up a firewall with ufw

We can check the status of ufw with the command

```
sudo ufw status verbose
```

It should return inactive if you have just installed ufw for the first time.

By default ufw is configured to:

- deny all incoming traffic
- allow all outgoing traffic

These are good defaults, we can confirm them with the two commands below

```
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

to configure ufw we can use _allow_, _deny_ and _limit_ commands

```
sudo ufw allow from 203.0.113.0/24 to any port 22
sudo ufw deny from 203.1.113.4
ufw limit ssh
```

In a production server we would probably want to only allow ssh connections from specific ip addresses. We would also likely use something like a bastion host, rather than making an ssh connection to our servers directly. To make things easier for us, we are going to allow ssh from anywhere (this is what we have been doing all along anyway).

```
sudo ufw allow ssh
```

UFW is pretty flexible in how it can be configured. We can generally use port numbers, services and even applications.

So we could allow ssh with either of these commands to allow ssh as well.

```
sudo ufw allow SSH
sudo ufw allow 22
```

You can see a list of application commands with

```
sudo ufw app list
```

Let's also rate limit ssh connections. This will deny an incoming address if they attempt 6 initiations in 30 seconds.

```
sudo ufw limit ssh
```

Since we are creating a web server we also want to allow http connections

```
sudo ufw allow http
```

Right now we have defined some rules for our firewall, but we haven't turned it on yet. We can turn our firewall on with this command:

```
sudo ufw enable
```

Finally, check the status again, to confirm that everything is working.

```
sudo ufw status verbose
```

You should see something like this:

```
To                         Action      From
--                         ------      ----
80/tcp                     ALLOW IN    Anywhere
22/tcp                     ALLOW IN    Anywhere
80/tcp (v6)                ALLOW IN    Anywhere (v6)
22/tcp (v6)                ALLOW IN    Anywhere (v6)
```

if you allow a service that you didn't mean to, you can remove it with

```
sudo ufw delete allow SSH
```

You can also delete a rule based on a number. First get numbered status

```
sudo ufw status numbered
```

This will return something that looks like this

```
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 22                         ALLOW IN    Anywhere                  
[ 2] 22 (v6)                    ALLOW IN    Anywhere (v6) 
```

If you want to remove rule 2

```
sudo ufw delete 2
```

ufw is quite flexible, You can configure more specific rules if needed.

**Resources:**

[https://www.cloudflare.com/learning/security/what-is-a-firewall/](https://www.cloudflare.com/learning/security/what-is-a-firewall/)

[https://wiki.archlinux.org/title/Uncomplicated_Firewall](https://wiki.archlinux.org/title/Uncomplicated_Firewall)

## [](#digitalocean-cloud-firewall)DigitalOcean Cloud Firewall

Most cloud service providers also offer some sort of firewall and other networking services. These generally have a few advantages over a tool like UFW.

One of the simplest reasons that you might use a cloud service providers firewall over something like ufw is that you can generally configure a firewall in a cloud service provider than put this in front of multiple servers.

**Reference:**

- [DigitalOcean Cloud Firewall Docs](https://docs.digitalocean.com/products/networking/firewalls/)

> [!Question] When would you use UFW instead of a cloud service providers firewall?