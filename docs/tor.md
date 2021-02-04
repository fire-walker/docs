## Tor Server Setup

Install tor<br>
`apt install tor`

Stop it's service which autostarted after installation<br>
`service tor stop`

Configure it here<br>
`/etc/tor/torcc`

Restart the service<br>
`service tor start`

The above automatically creates a hostname (onion url) with it's public and private key pair in the directory specified with the option `HiddenServiceDir` inside `torcc`.

Next create an nginx config for the site with the `ServerName` option set to the generated hostname

Nginx bucket size need to be increased as well



## References

[1] - https://www.bentasker.co.uk/documentation/linux/307-building-a-tor-hidden-service-from-scratch-part-1
<br>
[2] - https://2019.www.torproject.org/docs/tor-onion-service
<br>
[3] - https://github.com/alecmuffett/the-onion-diaries/blob/master/basic-production-onion-server.md 


