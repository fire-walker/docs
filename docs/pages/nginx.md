## Apache Configs

To convert `.htaccess` configs to nginx configs. Use [this](https://www.getpagespeed.com/apache-to-nginx) site.

## PHP Config

Here's a base nginx config for php. Make sure to change the php version and the `<url>`.

``` nginxconf
server {
    listen 80;
    listen [::]:80;
    server_name <url>;

    root /var/www/html;
    index index.php index.html index.htm index.nginx-debian.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php<version>-fpm.sock;
    }
}
```