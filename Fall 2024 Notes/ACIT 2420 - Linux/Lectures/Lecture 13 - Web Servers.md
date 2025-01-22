### Load Balancing 
- balances incoming traffic to web servers 

### what can you do with a load balancer?

#### [](#increased-availability-and-resilience)increased availability and resilience

If 100 people are looking at your site in a month, a single server is probably all that you need. If 100 000 000 are looking at your site in a month, 1 server will not be sufficient.

Even if you only have 100 people looking at your site in a month, you might want two servers, just in case one crashes.

#### [](#deployment-strategies)deployment strategies

A deployment strategy is exactly what it sounds like, a plan for how your are going to make your app available. You could just upload the newest version to all of your servers, this is sometimes referred to as "big bang" deployment.

There are several types of phased deployment, where you release the new version in phases, over a period of time that make use of a load balancer. For example "Canary" deployment.

**Resources:**

[https://devopsbootcamp.org/8-deployment-strategies-explained-and-compared/](https://devopsbootcamp.org/8-deployment-strategies-explained-and-compared/)

#### digital ocean load balancer set up

We are gong to setup a load balancer with digitalocean's load balancer service.

We will use tags to group our servers then create a load balancer that sits in front of all of the servers with a specific tag

**Resources:**

[https://www.digitalocean.com/products/load-balancer](https://www.digitalocean.com/products/load-balancer)

# reverse proxy server

> [!question]- What is a reverse proxy server A reverse proxy server sits in front of one or more servers and passes client requests to those servers, and sends their responses

Reverse proxy servers provide a layer of security for our backend servers. Clients never interact directly with them.

```
server {
    listen 80;
    listen [::]:80;
    
    root /var/www/my-site;
    
    index index.html;
    
    server_name _;
    
    location / {
        try_files $uri $uri/ =404;
    }
    
    location /<end-point> {
        # Define the reverse proxy settings
        proxy_pass <ip:port>;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

The 'proxy_set_header' directives are used to pass headers from the original request, to the proxy server.

The 'proxy_pass' directive is where the magic happens, this is the location of the actual server. This could be another physical server, or virtual machine, or something like a Python application running on the same server as your web server.

```
proxy_pass 127.0.0.1:5555
```

**Resources:**

[https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_header](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_set_header)

### setting up a file server in nginx

A file server or document server doesn't render the documents, like an HTML page. A file server shows a list of files that you can open or download. This is used less often today, with utilities like Google drive and remote version control repositories. It is still useful if you want to setup a very simple method of providing a common location to access documents for a small team.

```
server {
    listen 80;
    server_name your-server-name-or-ip;

    # Root directory for the file server
    location /files {
        root /path/to/your/files;
        autoindex on;                # Enables the directory listing
        autoindex_exact_size off;    # Shows file sizes, human-readable
        autoindex_localtime on;      # Displays file timestamps
    }

    # Default location for other requests (optional)
    location / {
        root /path/to/other/root;
        index index.html;
    }
}
```

### [](#note-about-the-root-directive-and-urls)Note about the `root` directive and URLS.

The path in location is appended to the root path This means that in the example below the path on your system where the files are located would be: `/var/www/html/files/`

```
location /files {
  root /var/www/html
}
```

Make sure you take this into account, or use the `alias` directive

In the example below the path would be: `/var/www/html/`

```
location /files {
  alias /var/www/html/
}  
```

**Reference:**

[https://nginx.org/en/docs/http/ngx_http_autoindex_module.html](https://nginx.org/en/docs/http/ngx_http_autoindex_module.html)

[https://docs.nginx.com/nginx/admin-guide/web-server/serving-static-content/](https://docs.nginx.com/nginx/admin-guide/web-server/serving-static-content/)

[https://nginx.org/en/docs/http/ngx_http_core_module.html#alias](https://nginx.org/en/docs/http/ngx_http_core_module.html#alias)

[https://nginx.org/en/docs/http/ngx_http_core_module.html#root](https://nginx.org/en/docs/http/ngx_http_core_module.html#root)