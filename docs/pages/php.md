## Version Mayhem

Start by deleting every php version

``` bash
sudo apt-get purge php7.*
sudo apt-get autoclean
sudo apt-get autoremove
```

Then follow a suitable guide and install the correct version

## Switching Active Version

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