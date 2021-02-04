## Setup

Setup a Mail Transfer Agent, for example [postfix](postfix.md).

Create `.local` copies of original configs residing in `/etc/fail2ban`
```
fail2ban.conf            => fail2ban.local
jail.conf                => jail.local
action.d/mail-whois.conf => mail-whois.local
```

Change mail formats
```
micro /etc/fail2ban/action.d/mail-whois.local
```

Prevent ufw conflicts
``` conf
/etc/fail2ban/jail.d/defaults-debian.conf
enabled=false
```


## Troubleshooting

!!! error "Ban issues"
    When encountering potential ufw conflicts resulting in _already banned_ errors.[^1] 

!!! warning "Firewall"
    Check ufw OpenSSH app. Because this is what gets banned in `jail.local`[^2] 



[^1]: https://askubuntu.com/questions/54771/potential-ufw-and-fail2ban-conflicts

[^2]: https://www.fail2ban.org/wiki/index.php/FAQ_english


