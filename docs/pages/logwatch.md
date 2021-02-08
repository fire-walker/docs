## Setup

Instal logwatch 

``` bash
apt-get install logwatch
```

Go to config location

``` bash
micro "/usr/share/logwatch/default.conf/"
```

Make a backup of the config file

``` bash
cp logwatch.conf backup.conf
```

Edit the file according to the references

[^1]: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-logwatch-log-analyzer-and-reporter-on-a-vps
[^2]: http://www.mewbies.com/how_to_install_and_configure_logwatch.html
