# Nginx Web Server Configuration

## Default NGINX Configuration

Recall that the default NGINX configuration is in /etc/nginx/sites-available/default:

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        # First attempt to serve request as file, then
        # as directory, then fall back to displaying a 404.
        try_files $uri $uri/ =404;
    }
}
```

This configures NGINX so that any hostname that is not otherwise configured will use /var/www/html for its files.

So right now, every subdomain you try should go to the same website in /var/www/html.

## Configuring NGINX for a specific subdomain

To setup NGINX so that it serves your custom subdomain from a different directory we need to create a new configuration file. 
We will be doing our work in the NGINX configuration directory:

```zsh
cd /etc/nginx/sites-available
```

Right now, you have one site available: "default". To create a new site:

```zsh
sudo touch subdomain.basedomain.org
```

Now edit this with vim:

```zsh
sudo vim subdomain.basedomain.org
```

Notice that since this file is owned by "root", you need to use "sudo" to edit it. Place the following in this file, but change the domain name (in two places) to the one that you use:

```
server {
  listen 80;
  server_name subdomain.basedomain.org;
  root /var/www/subdomain.basedomain.org;
  index index.html;
  default_type "text/html";
  location / {
    try_files $uri $uri/ =404;
  }
}
```

This file basically says: _If a connection comes into this machine for subdomain.basedomain.org, on port 80 (not encrypted), then look in /var/www/subdomain.basedomain.org for the files to serve._

The two important lines are the line for "server_name" and "root". The first gives the name of your website and the second provides the location where files for that website will be stored.

Now you need to enable it the new website configuration. You do this by creating a symbolic link (using the ln command):

```zsh
sudo ln -s /etc/nginx/sites-available/subdomain.basedomain.org /etc/nginx/sites-enabled/
```

This creates a link (or a shortcut) from /etc/nginx/sites-enabled/subdomain.basedomain.org to /etc/nginx/sites-available/subdomain.basedomain.org.

You need to make sure the new web site directory exists and that you own it:

```zsh
sudo mkdir /var/www/subdomain.basedomain.org
sudo chown jroberts /var/www/subdomain.basedomain.org
```

You now need to reload Nginx:

```zsh
sudo service nginx reload
```
