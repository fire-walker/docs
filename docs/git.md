## SSH Setup

To generate ssh keys, change `key-name` and `/absolute/path`
```
ssh-keygen -t rsa -b 4096 -C "key-name" -f "/absolute/path"
```

Check if ssh-agent is running<br>
`eval "$(ssh-agent -s)"`

Add generated ssh key to ssh-agent<br>
`ssh-add /key/location`

Then just copy the `.pub` and paste it into `github.com`