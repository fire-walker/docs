## Setup

Installing apache2. 

``` bash
sudo apt install apache2 libapache2-mod-phpx.x
```

For apache to handle php you need to edit the `/etc/apache2/mods-enabled/dir.conf`. And replace it's `<IfModule>` with;

``` apacheconf
<IfModule mod_dir.c>
     DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
```

Finally enable the apache php module and restart apache.

``` bash
sudo a2enmod phpx.x
sudo service apache2 restart
```

## Config

Config example for site with `.htaccess` files.

``` apacheconf
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
</VirtualHost>
```