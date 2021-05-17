## Commands

``` bash
# output linux version
cat "/proc/version"

# see last 10 logins
who wtmp | tail -10

# connect to a docker container terminal
docker exec -it <cont> bash

# find a keyword from an output
grep '<keyword(s)>'

# strip and output only the selected column
awk '{print $<col_num>}'

# info about a pid
ps -Flww -p <pid>
```

## Site Permissions

Site permissions when popping up a new site.

``` bash
sudo chown -R www-data:www-data loc/
sudo chmod -R 774 loc/
```

## Manual Fonts

For a single user place fonts in[^4];

``` sh
~/.local/share/fonts
```

For system-wide fonts place them in[^4];

``` sh
/usr/local/share/fonts

# don't touch this
# it's only for pacman
/usr/share/fonts/
```

## Sudo

One of the main things to do is to change the password asked when a user calls onto sudo. Making it ask for the root password is secure than the user pwd. So in the `/etc/sudoers` file;

``` properties
Defaults rootpw
```

Add a user to sudoers

``` bash
usermod -a -G sudo <username>
```

### Whitelisting

Whitelisting a command or set of commands for a group allows anyone who's in it to run them without `sudo`. This is done by creating a new file inside of `/etc/sudoers.d`[^2].

``` bash
sudo visudo -f "/etc/sudoers.d/<file-name>"
```

Inside it the following properties whitelists the app[^3]. Remember to give the abs path for the application. Use `which <app-name>` to find it.

``` properties
Cmnd_Alias <SET-NAME> = <abs-app-path> command, <apb-app-path> command
%<group-name> ALL=(ALL) NOPASSWD: <SET-NAME>
```

## System User

A user with no home directory, login shell nor password. It's basically a no-login dummy account made solely to containerize services.

``` bash
# create a system user and group of the same name
sudo useradd --system --no-create-home --shell=/sbin/nologin <username>

# set their permissions
sudo chown -R root:<username> /path/to/change
sudo chmod -R 775 /path/to/change
```

## chsh

If you ever get the error;

``` properties
chsh: PAM authentication failed
```

Find and comment this line inside `etc/pam.d/chsh`[^1].

``` properties
auth required pam_shells.so
```

Then do whatever you were doing and make sure to **uncomment it again**.

## Groups

List all groups

``` bash
getent group
```

Add user to a group

``` bash
usermod -a -G group <username>
```

What groups is a user in. If there's no args, groups of current user are shown.

``` bash
groups <user>
```

Create new group

``` bash
groupadd <group>
```

Make sure to restart the services that are responsible for the groups after adding a user into one.

Necessary groups for reg user;

- docker
- sudo
- postfix

## GPG

Decrypt a file.

``` bash
gpg --output <output-file> --decrypt <file.gpg>
```

Encrypt a file.

``` bash
gpg --output <output-file.gpg> --encrypt <file>
```

Export public and private keys.

``` bash
# public key
gpg --output <public.pgp> --armor --export -r <recipient>

# private key
gpg --output <private.pgp> --armor --export-secret-key -r <recipient>
```

[^1]: https://ubuntuforums.org/showthread.php?t=1702833
[^2]: https://askubuntu.com/questions/930768/adding-local-content-in-etc-sudoers-d-instead-of-directly-modifying-sodoers-fi
[^3]: https://askubuntu.com/questions/692701/allowing-user-to-run-systemctl-systemd-services-without-password
[^4]:https://wiki.archlinux.org/title/Fonts#Manual_installation