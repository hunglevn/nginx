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
