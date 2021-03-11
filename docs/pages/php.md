## Version Mayhem

Start by deleting every php version

``` bash
sudo apt-get purge php7.*
sudo apt-get autoclean
sudo apt-get autoremove
```

Then follow a suitable guide and install the correct version

### Switching Active Version

Enabling and disabling a specific version

``` bash
# disable
sudo a2dismod php7.0

# enable
sudo a2enmod php7.2
```

Then rerun web server

``` bash
service apache2 restart
service nginx restart
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