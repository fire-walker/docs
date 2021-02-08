## Pre-setup

Create mailgun flex account at `https://www.mailgun.com`. Then make an SMTP user and take down these.

``` bash
username (name@domain.com / postmaster@domain.com)
password
smtp hostname (smtp.eu.mailgun.org / smtp.us.mailgun.org)
```

## Postfix Setup

Firstly install postfix. On the tui that's displayed select `internet site` and enter your FQDN.

``` bash
sudo apt-get update && sudo apt-get install postfix
```

If you don't get an interactive prompt. Fully remove and reinstall postfix.

``` bash
sudo apt purge postfix && sudo apt-get install postfix
```

Add the regular user to the postfix group to send mails without auth problems

``` bash
usermod -a -G postfix <username>
```

After installation edit postfix config file at `/etc/postfix/main.cf`. Find suitable settings from the reference[^1]

``` properties
mydestination = localhost.domain, localhost
relayhost = [<SMTP hostname>]:587
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = static:<postmaster@domain.com>:<password>
smtp_sasl_security_options = noanonymous

smtp_tls_security_level = may
smtpd_tls_security_level = may
smtp_tls_note_starttls_offer = yes

# domain mapper
smtp_generic_maps = hash:/etc/postfix/generic
```

To customize the domain name mails are sent as, a domain mapper must be created for each user. Create a new file `/etc/postfix/generic`. In it add records for each user. The first part identifies the user in the host server. The second part is what's displayed in the email's from section.

``` bash
username@hostname sender@domain
someone@zen noreplay@thelonelylands.com
```

Then to initialize change run

``` bash
sudo postmap /etc/postfix/generic
sudo service postfix reload
```

Postfix configs are errorless if the result is none

``` bash
sudo postfix check
sudo service postfix reload
```

!!! tip ""
    Further you check runtime logs for errors

    ``` bash
    service postfix status ==> active(exited)
    ```

!!! tip ""
    Finally test it using `sendmail`
    ``` bash
    echo "Some text" | sendmail <recipient-email>
    ```


## Troubleshooting

!!! warning ""
    If you encounter the warning below when checking config. Just act as if you didn't see it[^2]

    ``` properties
    postfix/postfix-script: warning: symlink leaves directory: /etc/postfix/./makedefs.out
    ```

!!! error ""
    The `535 Authentication Error` usually implies an error in the data we give. May it be a password, a link or the hostname.

[^1]: https://documentation.mailgun.com/en/latest/user_manual.html#smtp-relay
[^2]: https://serverfault.com/questions/1004137/postfix-postfix-script-warning-symlink-leaves-directory-etc-postfix-makedefs
