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
# Create a site for serving static contain:
Below is an example to create a `foo` site listen on port number 8000.
- Create configuration file `"/etc/nginx/sites-available/foo"`
    - `sudo echo | sudo tee -a /etc/nginx/sites-available/foo`
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
- Verify that site is up and running: `curl -I 127.0.0.1:8000`
