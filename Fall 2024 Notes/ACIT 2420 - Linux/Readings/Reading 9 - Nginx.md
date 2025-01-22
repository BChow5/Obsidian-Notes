- [nginx](https://en.wikipedia.org/wiki/nginx "wikipedia:nginx") (pronounced "engine X"), is a free, open-source, high-performance HTTP [web server](https://wiki.archlinux.org/title/Web_server "Web server") and reverse proxy, as well as an IMAP/POP3 proxy server
	- written by Igor Sysoev in 2005

### Installation
[Install](https://wiki.archlinux.org/title/Install "Install") one of the following packages:

- [nginx-mainline](https://archlinux.org/packages/?name=nginx-mainline) - mainline branch: new features, updates, bugfixes.
- [nginx](https://archlinux.org/packages/?name=nginx) - stable branch: major bugfixes only.
- [angie](https://aur.archlinux.org/packages/angie/)AUR - fork and drop-in replacement for nginx with more features.
- [freenginx-mainline](https://aur.archlinux.org/packages/freenginx-mainline/)AUR - drop-in replacement that preserves the free and open development of nginx (mainline branch).
- [freenginx-libressl](https://aur.archlinux.org/packages/freenginx-libressl/)AUR - drop-in replacement that preserves the free and open development of nginx (mainline branch with [LibreSSL](https://wiki.archlinux.org/title/LibreSSL "LibreSSL") support).
- [freenginx](https://aur.archlinux.org/packages/freenginx/)AUR - drop-in replacement that preserves the free and open development of nginx (stable branch).

Using the mainline branch is recommended. The main reason to use the stable branch is that you are concerned about possible impacts of new features, such as incompatibility with third-party modules or the inadvertent introduction of bugs in [new features](https://nginx.org/en/download.html).

### Running
- [Start/enable](https://wiki.archlinux.org/title/Start/enable "Start/enable") `nginx.service` or `angie.service` if you use Angie.\

### Configuration
- You can modify the configuration by editing the files in `/etc/nginx/` The main configuration file is located at `/etc/nginx/nginx.conf`.

#### Example
```bash
user http;
worker_processes auto;
worker_cpu_affinity auto;

events {
    multi_accept on;
    worker_connections 1024;
}

http {
    charset utf-8;
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    server_tokens off;
    log_not_found off;
    types_hash_max_size 4096;
    client_max_body_size 16M;

    # MIME
    include mime.types;
    default_type application/octet-stream;

    # logging
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log warn;

    # load configs
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

#### General configuration
- You should choose a fitting value for `worker_processes`
	- This setting ultimately defines how many connections nginx will accept and how many processors it will be able to make use of.
- Generally, making it the number of hardware threads in your system is a good start. 
- Alternatively, `worker_processes` accepts the `auto` value since versions 1.3.8 and 1.2.5, which will try to autodetect the optimal value
- The maximum connections nginx will accept is given by `max_clients = worker_processes * worker_connections`

#### Running under different user
By default, [nginx](https://archlinux.org/packages/?name=nginx) runs the master process as `root` and worker processes as user `http`. To run worker processes as another user, change the `user` directive in `nginx.conf`:

`/etc/nginx/nginx.conf`

`user _user_ [group];`

If the group is omitted, a group whose name equals that of _user_ is used.

#### Server blocks
- It is possible to serve multiple domains using `server` blocks. These are comparable to "VirtualHosts" in [Apache HTTP Server](https://wiki.archlinux.org/title/Apache_HTTP_Server "Apache HTTP Server").
- In the example below the server listens for incoming connections on IPv4 and IPv6 ports 80 for two domains, `domainname1.tld` and `domainname2.tld`:

`/etc/nginx/nginx.conf`
```bash
...
server {
    listen 80;
    listen [::]:80;
    server_name domainname1.tld;
    root /usr/share/nginx/domainname1.tld/html;
    location / {
        index index.php index.html index.htm;
    }
}

server {
    listen 80;
    listen [::]:80;
    server_name domainname2.tld;
    root /usr/share/nginx/domainname2.tld/html;
    ...
}
```
- Make sure the hostnames are resolvable by setting up a DNS-server like [BIND](https://wiki.archlinux.org/title/BIND "BIND") or [dnsmasq](https://wiki.archlinux.org/title/Dnsmasq "Dnsmasq"), or have a look at [Network configuration#Local network hostname resolution](https://wiki.archlinux.org/title/Network_configuration#Local_network_hostname_resolution "Network configuration").

##### Managing server entries

It is possible to put different `server` blocks in different files. This allows you to easily enable or disable certain sites.

![](https://wiki.archlinux.org/images/0/0b/Inaccurate.svg)**The factual accuracy of this article or section is disputed.**

**Reason:** It is contested if the below approach using `sites-enabled` and `sites-available` is still useful and doesn't create more problems, see [comparing the two approaches](https://serverfault.com/questions/527630/difference-in-sites-available-vs-sites-enabled-vs-conf-d-directories-nginx) and [example of problems arising through sites-enabled and sites-available approach](https://stackoverflow.com/questions/45660042/nginx-proxy-pass-leads-to-404-not-found-page/45789055).

Instead, one can just create files inside `etc/nginx/conf.d/` which adheres to the standard of drop in configuration files. Then, include `include /etc/nginx/conf.d/*.conf` in the main config file, similar to including other file patterns in other directories as shown below. This way, sites can be disabled just be renaming them to e.g. `original_name.conf.disabled`, since only files ending in _.conf_ are included.

(Discuss in [Talk:Nginx](https://wiki.archlinux.org/title/Talk:Nginx))

For using the `sites-enabled` and `sites-available` approach, create the following directories:

```
# mkdir /etc/nginx/sites-available
# mkdir /etc/nginx/sites-enabled
```

Create a file inside the `sites-available` directory that contains one or more server blocks:

/etc/nginx/sites-available/example.conf

```
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    http2 on;

    ...
}
```

Append `include sites-enabled/*;` to the end of the `http` block:

```
/etc/nginx/nginx.conf

http {
    ...
    include sites-enabled/*;
}
```

To enable a site, simply create a symlink:

# ```
```
ln -s /etc/nginx/sites-available/example.conf /etc/nginx/sites-enabled/example.conf
```

To disable a site, unlink the active symlink:

`# unlink /etc/nginx/sites-enabled/example.conf`

[Reload](https://wiki.archlinux.org/title/Reload "Reload")/[restart](https://wiki.archlinux.org/title/Restart "Restart") `nginx.service` to enable changes to the site's configuration.

###  FastCGI

[FastCGI](https://en.wikipedia.org/wiki/FastCGI "wikipedia:FastCGI"), also FCGI, is a protocol for interfacing interactive programs with a web server. FastCGI is a variation on the earlier [Common Gateway Interface](https://en.wikipedia.org/wiki/Common_Gateway_Interface "wikipedia:Common Gateway Interface") (CGI); FastCGI's main aim is to reduce the overhead associated with interfacing the web server and CGI programs, allowing servers to handle more web page requests at once.

FastCGI technology is introduced into nginx to work with many external tools, e.g. [Perl](https://wiki.archlinux.org/title/Perl "Perl"), [PHP](https://wiki.archlinux.org/title/PHP "PHP") and [Python](https://wiki.archlinux.org/title/Python "Python").

#### PHP implementation

[PHP-FPM](https://php-fpm.org/) is the recommended solution to run as FastCGI server for [PHP](https://wiki.archlinux.org/title/PHP "PHP").

[Install](https://wiki.archlinux.org/title/Install "Install") [php-fpm](https://archlinux.org/packages/?name=php-fpm) and make sure [PHP](https://wiki.archlinux.org/title/PHP "PHP") has been installed and configured correctly. The main configuration file of PHP-FPM is `/etc/php/php-fpm.conf`. For basic usage the default configuration should be sufficient.

Finally, [start/enable](https://wiki.archlinux.org/title/Start/enable "Start/enable") `php-fpm.service`.

You can also use [php-legacy-fpm](https://archlinux.org/packages/?name=php-legacy-fpm) instead, see [#Using php-legacy](https://wiki.archlinux.org/title/Nginx#Using_php-legacy).

**Note:**

- If you [run nginx under a different user](https://wiki.archlinux.org/title/Nginx#Running_under_different_user), make sure that the PHP-FPM socket file is accessible by this user, or use a TCP socket.
- If you run nginx in chrooted environment (chroot is `/srv/nginx-jail`, web pages are served at `/srv/nginx-jail/www`), you must modify the file `/etc/php/php-fpm.conf` to include the `chroot = /srv/nginx-jail` and `listen = /srv/nginx-jail/run/php-fpm/php-fpm.sock` directives within the pool section (a default one is `[www]`). Create the directory for the socket file, if missing. Moreover, for modules that are dynamically linked to dependencies, you will need to copy those dependencies to the chroot (e.g. for php-imagick, you will need to copy the ImageMagick libraries to the chroot, but not imagick.so itself).

##### nginx configuration

When serving a PHP web-application, a `location` for PHP-FPM should to be included in each [server block](https://wiki.archlinux.org/title/Nginx#Server_blocks) [[2]](https://www.nginx.com/resources/wiki/start/topics/examples/phpfcgi/), e.g.:

/etc/nginx/sites-available/example.conf

```
server {
    root /usr/share/nginx/html;

    location / {
        index index.html index.htm index.php;
    }

    location ~ \.php$ {
        # 404
        try_files $fastcgi_script_name =404;

        # default fastcgi_params
        include fastcgi_params;

        # fastcgi settings
        fastcgi_pass			unix:/run/php-fpm/php-fpm.sock;
        fastcgi_index			index.php;
        fastcgi_buffers			8 16k;
        fastcgi_buffer_size		32k;

        # fastcgi params
        fastcgi_param DOCUMENT_ROOT	$realpath_root;
        fastcgi_param SCRIPT_FILENAME	$realpath_root$fastcgi_script_name;
        #fastcgi_param PHP_ADMIN_VALUE	"open_basedir=$base/:/usr/lib/php/:/tmp/";
    }
}
```

If it is needed to process other extensions with PHP (e.g. _.html_ and _.htm_):

```
location ~ [^/]\.(php|html|htm)(/|$) {
    ...
}
```

Non _.php_ extension processing in PHP-FPM should also be explicitly added in `/etc/php/php-fpm.d/www.conf`:

`security.limit_extensions = .php .html .htm`

**Note:** Pay attention to the `fastcgi_pass` argument, as it must be the TCP or Unix socket defined by the chosen FastCGI server in its configuration file. The **default** (Unix) socket for `php-fpm` is:

`fastcgi_pass unix:/run/php-fpm/php-fpm.sock;`

You might use the common TCP socket, **not default**,

`fastcgi_pass 127.0.0.1:9000;`

Unix domain sockets should however be faster.

**Tip:** To allow multiple `server` blocks using the same PHP-FPM configuration, a `php_fastcgi.conf` configuration file may be used to ease management:

/etc/nginx/php_fastcgi.conf

```
location ~ \.php$ {
    # 404
    try_files $fastcgi_script_name =404;

    # default fastcgi_params
    include fastcgi_params;

    # fastcgi settings
    ...
}
```

To enable PHP support for a particular server, simply include the `php_fastcgi.conf` configuration file:

/etc/nginx/sites-available/example.conf

```
server {
    server_name example.com;
    ...

    include /etc/nginx/php_fastcgi.conf;
}
```