<div id="top"></div>

# Deploying an app with Nginx & Let's Encrypt

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


```nginx
```

### CNAME record

An example of a CNAME type DNS record config file, where we want the domain
`mydomain` to redirect the request into the `mydomaintoredirectto` server name.

```nginx
```

<p align="right">(<a href="#top">go to top</a>)</p>