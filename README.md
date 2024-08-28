<div id="top"></div>

# Deploying an app with Nginx, PM2 & Let's Encrypt

<!-- TABLE OF CONTENTS -->
<details>
    <summary>Table of Contents</summary>
    <ol>
        <li>
            <a href="#getting-started">Getting Started</a>
            <ul>
                <li><a href="#prerequisites">Prerequisites</a></li>
                <li><a href="#installation">Installation</a></li>
            </ul>
        </li>
        <li>
            <a href="#usage">Usage</a>
            <ul>
                <li><a href="#server">Server</a></li>
                <li><a href="#running">Running</a></li>
                <li><a href="#certificate">Certificate</a></li>
            </ul>
        </li>
        <li>
            <a href="#examples">Examples</a>
            <ul>
                <li><a href="#a-record">A record</a></li>
                <li><a href="#cname-record">CNAME record</a></li>
            </ul>
        </li>
    </ol>
</details>

## Getting started

### Prerequisites

- [npm](https://nodejs.org/en/download/package-manager/)

### Installation

Install `nginx` from your package manager:

```bash
sudo apt install nginx
```

<p align="right">(<a href="#top">go to top</a>)</p>

## Usage

### Server

1. Go to the path `/etc/nginx/sites-available/`.

2. Once there, create a file for each subdomain the web page has.

3. To enable it, create a symbolic link of the file in `/etc/nginx/sites-enabled/`
    ```bash
    sudo ln -s /etc/nginx/sites-available/mydomain /etc/nginx/sites-enabled/
    ```

4. Verify the new configuration is correct.
    ```bash
    sudo nginx -t
    ```

5. Restart the nginx server.
    ```bash
    sudo service nginx restart
    ```

### Running

To have the server open while we're logged out of the shell, we'll use the
daemon [PM2](https://pm2.io).

1. Install PM2.
    ```bash
    npm install pm2 -g
    ```

2. Go to the server's path and run it using PM2. Choose a name to substitute
MyAppName for.
    ```bash
    cd path/to/my/server
    pm2 start index.js --name MyAppName
    ```

3. Use `pm2 ls` to show running apps, `pm2 stop 'index/name'` to stop an app,
`pm2 start 'index/name'` to start the app again, `pm2 delete 'index/name'` to
remove the app from the daemon, and `pm2 logs 'index/name'` to inspect the logs
of an app.

### Certificate

A certificate can be created for the web page to use the HTTPS protocol. For
that, we'll use Let's Encrypt's certbot:

1. Install certbot.
    ```bash
    sudo snap install core
    sudo snap refresh core
    sudo snap install --classic certbot
    ```

2. Have your web app active and accesible.

3. Run certbot for the configured domain.
    ```bash
    sudo certbot --nginx -d mydomain
    ```

<p align="right">(<a href="#top">go to top</a>)</p>

## Examples

### A record

An example of a A type DNS record config file, where an app is listening in port
`3000`, and the server name is `mydomain`.

<!-- MARKDOWN-AUTO-DOCS:START (CODE:src=./examples/a.nginx) -->
<!-- The below code snippet is automatically added from ./examples/a.nginx -->
```nginx
server {
    server_name mydomain;

    listen 80;
    listen [::]:80;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```
<!-- MARKDOWN-AUTO-DOCS:END -->

### CNAME record

An example of a CNAME type DNS record config file, where we want the domain
`mydomain` to redirect the request into the `mydomaintoredirectto` server name.

<!-- MARKDOWN-AUTO-DOCS:START (CODE:src=./examples/cname.nginx) -->
<!-- The below code snippet is automatically added from ./examples/cname.nginx -->
```nginx
server {
    server_name mydomain;

    listen 80;
    listen [::]:80;

    return 301 $scheme://mydomaintoredirectto$request_uri;
}
```
<!-- MARKDOWN-AUTO-DOCS:END -->

<p align="right">(<a href="#top">go to top</a>)</p>