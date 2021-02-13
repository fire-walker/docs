## Commands
``` bash
# fail2ban server status
sudo service fail2ban stastus

# see jail status
sudo fail2ban-client status ssh

# manually ban ip
sudo fail2ban-client set sshd banip 23.34.45.56

# manually unban ip
sudo fail2ban-client set sshd unbanip 23.34.45.56
```


## Installation
Setup a Mail Transfer Agent, for example [postfix](postfix.md).

Then install `fail2ban`. Make sure stop its automatically started service.

``` bash
sudo apt install fail2ban
sudo service fail2ban stop
```

## Configuration

Fail2ban reads files from two configuration formats. Files ending with the suffixes `.conf` (original) and `.local` (custom) within `/etc/fail2ban`. The default config files of fail2ban come saved with the suffix `.conf`. 

Never touch these files. Modifying them is not recommended cos there's a possibility of them being overwritten after updates. Always make changes after creating a copy of the with the `.local` suffix.

The `.local` file doesn’t have to include all settings from the corresponding `.conf` file, only those that need to be overridden.

The configs below are meant to be self sufficient. Meaning the files don't need extra rules other than the ones stated to function as intended.

``` properties
fail2ban.conf               => fail2ban.local
jail.conf                   => jail.local
action.d/mail-whois.conf    => mail-whois.local
jail.d/defaults-debian.conf => defaults-debian.local
```

---

#### defaults-debian.local

Prevent ufw conflicts.

``` ini
[sshd]
enabled = false
```

---

#### fail2ban.local

Change log level.

``` ini
[DEFAULT]
loglevel = INFO
```

---

#### jail.local

Setup the main jail which fail2ban would be listening to and blocking ips.

``` ini
[ssh]
enabled = true
port = <ssh-port>
filter = sshd
logpath = /var/log/auth.log
maxretry = 4
bantime = 1w
findtime = 1d

destemail = <dest-email>
sender = fail2ban
mta = mail
chain = <known/chain>

# basically what happens when blocking
action_mwl = ufw[application="OpenSSH", blocktype=reject]
	         %(mta)s-whois[name=%(__name__)s, sender="%(sender)s", dest="%(destemail)s", protocol="%(protocol)s", chain="%(chain)s"]

action = %(action_mwl)s
```

---

#### mail-whois.local

To configure the mail templates.

``` ini
[INCLUDES]
before = mail-whois-common.conf

[Definition]
norestored = 1

actionstart = printf %%b "Hi,\n
              The jail <name> has been started successfully.\n
              Regards,\n
              Fail2Ban"|mail -s "Fail2Ban: Started" <dest>

actionstop = printf %%b "Hi,\n
             The jail <name> has been stopped.\n
             Regards,\n
             Fail2Ban"|mail -s "Fail2Ban: Stopped" <dest>

actionban = printf %%b "Hi,\n
            The IP <ip> has just been banned by Fail2Ban after
            <failures> attempts against <name>.\n\n
            Here is more information about <ip> :\n
            `%(_whois_command)s`\n
            Regards,\n
            Fail2Ban"|mail -s "Code Yellow: SSH banned ip" <dest>

[Init]
name = default
# addressee of the mail
dest = fail2ban

```

## Troubleshooting

!!! error ""
 When encountering potential ufw conflicts resulting in _already banned_ errors. Check[^1]

!!! warning ""
    Check ufw's OpenSSH app. Because this is what gets banned in `jail.local`[^2]
   
[^1]: https://askubuntu.com/questions/54771/potential-ufw-and-fail2ban-conflicts
[^2]: https://www.fail2ban.org/wiki/index.php/FAQ_english
