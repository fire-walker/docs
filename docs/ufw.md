## Applications

Put the apps at `/etc/ufw/applications.d/`

``` conf
[appname]
title=1-liner here
description=a longer line here
ports=1,2,3,4,5,6,7,8,9,10,30/tcp|50/udp|53

[SMTP Ports]
title=Ports for mail client
description=Most possibly this would work. I guess
ports=25,465,587,110,995,143,993/tcp
```

!!! example "Necessary Apps"
    - OpenSSH - 7777
    - Nginx - 80, 443
    - SMTP Ports - 587



## Default rules
``` bash
ufw default deny incoming
ufw default allow outgoing
```


## Commands

| Syntax                   | Description          |
| -----------              | -----------          |
| `ufw allow "SMTP Ports"` | allowing an app      |
| `ufw delete [num]`       | deleting a record    |