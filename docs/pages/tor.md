## Tor Server Setup

Install tor and stop it's auto started service.

``` bash
sudo apt install tor && sudo service tor stop
```


Then edit it's config file `/etc/tor/torcc`

``` properties
SocksPort 0
SocksListerAddress 127.0.0.1

RunAsDaemon 1
DataDirectory /var/lib/tor

HiddenServiceDir /var/lib/tor/my_onion_service
HiddenServicePort 80 127.0.0.1:8921
```

After configurations start tor.

``` bash
service tor start
```

## Info

The above automatically creates a hostname (onion url) with it's public and private key pair in the directory specified with the option `HiddenServiceDir` inside `torcc`.

Next create an nginx config for the site with the `ServerName` option set to the generated hostname

## Troubleshooting

!!! error ""
    Nginx bucket size needs to be increased. The length of a tor address overshoots default config limitations

[^1]: https://www.bentasker.co.uk/documentation/linux/307-building-a-tor-hidden-service-from-scratch-part-1
[^2]: https://2019.www.torproject.org/docs/tor-onion-service
[^3]: https://github.com/alecmuffett/the-onion-diaries/blob/master/basic-production-onion-server.md 
