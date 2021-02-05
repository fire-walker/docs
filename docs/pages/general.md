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

# port usage
netstat -pnltu
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
usermod -a -G group username
```

Make sure to restart the services that are responsible for the groups after adding a user into one.

Necessary groups;

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
