## Setup

Install apache2 and the php plugins. Make sure that phpx.x-fpm is already installed. Because we're using fpm the original phpx.x package is not needed. `libapache2-mod-fcgid` enables apache to run the php server
with FPM.

```bash
sudo apt install apache2 libapache2-mod-fcgid
```

For apache to handle php you need to edit the `/etc/apache2/mods-enabled/dir.conf`. And replace it's `<IfModule>` with this.

```apacheconf
<IfModule mod_dir.c>
     DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
```

Now enable the apache php module and FastCGI config module.

```bash
sudo a2enmod actions fcgid alias proxy_fcgi setenvif
sudo a2enconf phpx.x-fpm

# don't forget
sudo service apache2 restart
```

Make sure to add this line to all site configs for php scripting support.

```apacheconf
<FilesMatch \.php$>
    SetHandler "proxy:unix:/var/run/php/phpx.x-fpm.sock|fcgi://localhost"
</FilesMatch>
```

## Config

Config example for site with `.htaccess` files.

```apacheconf
<VirtualHost *:80>
    ServerAdmin webmaster@example.com
    ServerName example.com
    DocumentRoot /var/www/loc

    <Directory /var/www/loc>
        Options Indexes FollowSymLinks MultiViews
        AllowOverride All
        Require all granted
        allow from all
        LimitRequestBody 5242880
    </Directory>

    <FilesMatch \.php$>
        SetHandler "proxy:unix:/var/run/php/phpx.x-fpm.sock|fcgi://localhost"
    </FilesMatch>
</VirtualHost>
```

## Behind Nginx

To run both Apache and Nginx at the same time, you need to expose one to the net and run the other through a proxy.

First there's Nginx which listens to port 80. It acts as the gateway to both Nginx native requests and Apache requests. Apache on the other hand is listening on a different port. Any request going
towards Apache will be forwarded to it's port by an Nginx config which will then be picked up by it's corresponding resolver on Apache's side.

### Steps

-   [x] Check PHP versions
-   [x] Install Apache
-   [x] Setup Apache
-   [x] Change Apache port
-   [x] Configure Nginx proxy
-   [x] Check if all's well

Check Apache installation, PHP installation, PHP version and if all the related modules exist and are enabled[^1]. Then add the apache modules to run php FPM[^2].

Configure Apache to listen to the different port. It may be any port you wish. Just make sure to never open this port to the internet. This can be done by changing the listening port at
`/etc/apache2/ports.conf`.

```apache
Listen 8080
```

The port should be mentioned in the virtual host of every site config at `/etc/apache2/sites-enabled` as well.

```apache
<VirtualHost *:8080>
    # config
</VirtualHost>
```

As for the proxy. An Nginx site config is needed with the following content.

```nginx
server {
    listen 80;
    server_name <apache.domain>;

    location / {
        proxy_pass http://<server_ip>:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Finally check if everything works fine by popping up a php test file. More info [here](../php/#testing). Check the Server API field to see if FastCGI is active.

|                  |                   |
| ---------------- | ----------------- |
| Build Date       | -                 |
| {==Server API==} | {==FPM/FastCGI==} |
| Virtual..        | -                 |

[^1]: https://www.digitalocean.com/community/tutorials/how-to-configure-nginx-as-a-web-server-and-reverse-proxy-for-apache-on-one-ubuntu-20-04-server
[^2]: https://tecadmin.net/setup-apache-php-fpm-ubuntu-20-04/
