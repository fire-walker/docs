## Version Mayhem

Start by deleting every php version
```
sudo apt-get purge php7.*
sudo apt-get autoclean
sudo apt-get autoremove
```

Follow a suitable guide and install the correct version


## Switching Acive Version

Enabling and disabling a specific version
```
# disable
sudo a2dismod php7.0

# enable
sudo a2enmod php7.2
```

Then rerun apache or nginx
```
service apache2 restart
service nginx restart
```