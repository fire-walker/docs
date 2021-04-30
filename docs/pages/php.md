## Version Mayhem

Start by deleting every php version

``` bash
sudo apt-get purge php7.*
sudo apt-get autoclean
sudo apt-get autoremove
```

Then follow a suitable guide and install the correct version.

### Switching Active Version

Enabling and disabling a specific version.

For plain php;

``` bash
# disable
sudo a2dismod php7.0

# enable
sudo a2enmod php7.2
```

For php-fpm;

``` bash
# disable
sudo service phpx.x-fpm stop

# enable
sudo service phpx.x-fpm start
```

Then rerun web server

``` bash
sudo service apache2 restart
sudo service nginx restart
```

## Plugins

When installing plugins don't just install it by it's name. You must specify the php version as well. For example when trying to install the php-xml plugin. You must run;

``` bash
sudo apt install php-xml
```

This would install the plugin for the most recent version of php. Which may or may not be the version you have running. So, first check the php version you have running using `php --version`. Then install the package that's relevant to it.

``` bash
# for php7.4
sudo apt install php7.4-xml
```

## Testing

Create the directory `/var/www/php-test` and place an `index.php` file with;

``` php
<?php
  phpinfo();
?>
```

## Config Location

Usually there's multiple php config files that exist. So you must figure out which file you're using. Then again, you could even be using multiple at the same time. 

However, if you're running both apache and nginx at the same time. There's a possibility that the two are using different `php.ini`'s.

First create a [php-test](../php/#testing) file. Then serve it with the preferred web server.

### Apache

Create a new conf file at `/etc/apache2/sites-available` and point it towards the test file. 

Remember to route the traffic if you're using an nginx reverse proxy to serve apache requests with nginx requests. If so, you'll need to customize the apache config at `/etc/nginx` as well.

### Nginx

Create a config file at `/etc/nginx` with the usual php stuff. Then point it towards the test file.



## ERROR - 413

The main causes for `413 files too large` are;

1. The web server limits it
2. PHP limits it

### Web Server Limitations

#### Apache

Incease the `LimitRequestBody`.
It can be used in any virtualhost or location directive. Check [here](https://httpd.apache.org/docs/2.2/mod/core.html#limitrequestbody) for more info. You need to convert whatever request size you need into bytes. Use [this](https://convertlive.com/u/convert/megabytes/to/bytes#10) to convert.

``` apacheconf
<Directory "/var/www/site">
	LimitRequestBody  5242880
</Directory>
```

Afterwords remember to restart apache.

``` bash
sudo service apache2 reload
```

#### Nginx

This is easy. Just edit the nginx config.

``` bash
sudo nano /etc/nginx/nginx.conf
```

Then change this to whatever size you want.

``` nginxconf
client_max_body_size 2M;
```

Afterwords remember to restart nginx.

``` bash
sudo service nginx reload
```

### PHP Limitations

First figure out which php `.ini` file is [read](../php#config-location).

In it, find and change the below parameters to the size you require.

``` ini
upload_max_filesize = 100M
post_max_size = 100M
```

Afterwords remember to restart the web server and php.

``` bash
sudo service apache2 restart
sudo service nginx restart
sudo service phpx.x-fpm restart
```
