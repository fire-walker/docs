## Commands

List all groups
``` bash
getent group
```

Add user to a group
``` bash
usermod -a -G group username
```

Make sure to restart the services that are responsible for the groups after adding a user into one.


## Necessary

!!! info ""
    - docker - to user docker without sudo
    - sudo - obvious
    - postfix - to send mail without sudo
