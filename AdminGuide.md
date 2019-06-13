# Install NGINX on Ubuntu
- `sudo apt-get update`
- `sudo apt-get install nginx`
- Verify installation `sudo nginx -v`
- Start NGINX: `sudo nginx`
- Verify that NGINX is up and running: `curl -I 127.0.0.1`
# Controlling NGINX
- `nginx -s <SIGNAL>`. Where `<SIGNAL>` can be one of the following:
    - `quit` – Shut down gracefully
    - `reload` – Reload the configuration file
    - `reopen` – Reopen log files
    - `stop` – Shut down immediately (fast shutdown)
# Create a site for serving static content:
Below is an example to create a `foo` site listen on port number 8000.
- Create configuration file `"/etc/nginx/sites-available/foo"`
    - Content of `foo` file:
        ```
        server {
            listen 8000;
            server_name localhost;
            root /usr/share/foo/html;
            index index.html index.htm;
            location / {
                try_files $uri $uri/ =404;
            }
        }
        ```
- To enalbe the site, create a link to `foo` file under `/etc/nginx/sites-enabled`
    - `ln -s /etc/nginx/sites-available/foo /etc/nginx/sites-enabled/foo`
- Create index file of site
    - `sudo echo 'Hello Foo' | sudo tee -a /usr/share/foo/html/index.html`
- Reload nginx: `sudo nginx -s reload`
- Verify that site is up and running: `curl -I 127.0.0.1:8000`
# Setting Up a Simple Proxy Server
For passing a request to an HTTP proxied server, the proxy_pass directive is specified inside a location. Beside HTTP, Nginx also support these non-HTTP proxied server:
- `fastcgi_pass` passes a request to a FastCGI server
- `uwsgi_pass` passes a request to a uwsgi server
- `scgi_pass` passes a request to an SCGI server
- `memcached_pass` passes a request to a memcached server
## To pass a request to an HTTP proxied server, the proxy_pass directive is specified inside a location
```
location /some/path/ {
    proxy_pass http://www.example.com/link/;
}
```
## To modiy header fields, use the `proxy_set_header` directive:
```
location /some/path/ {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_pass http://localhost:8000;
}
```
## Configuring Buffers
By default NGINX buffers responses from proxied servers. A response is stored in the internal buffers and is not sent to the client until the whole response is received.
The directive that is responsible for enabling and disabling buffering is `proxy_buffering`. By default it is set to `on` and buffering is enabled. To disable buffering, change value of `proxy_buffering` to `off`
```
location /some/path/ {
    proxy_buffering on;
    proxy_buffers 16 4k;
    proxy_buffer_size 2k;
    proxy_pass http://localhost:8000;
}
```
## Choosing an Outgoing IP Address
If proxy server has several network interfaces, we could choose a particular source IP address for connecting to a proxied server or an upstream.
```
location /app1/ {
    proxy_bind 127.0.0.1;
    proxy_pass http://example.com/app1/;
}

location /app2/ {
    proxy_bind 127.0.0.2;
    proxy_pass http://example.com/app2/;
}
```
## Below example proxy `/vnx` and `/goo` requests to an `vnexpress.net` and `google.com` servers.
- Create configuration file `"/etc/nginx/sites-available/proxy"`
    - Content of `proxy` file:
        ```
        server {
            listen 8001;
            location /vnx {
                proxy_pass https://vnexpress.net/;
            }
            location /goo {
                proxy_pass https://google.com/;
            }
        }
        ```
- To enable above proxy configuration file, create a link to `proxy` file under `/etc/nginx/sites-enabled`
    - `ln -s /etc/nginx/sites-available/proxy /etc/nginx/sites-enabled/proxy`
- Reload nginx: `sudo nginx -s reload`
- Verify that site is up and running: `curl -I 127.0.0.1:8001/vnx`, `curl -I 127.0.0.1:8001/goo`



