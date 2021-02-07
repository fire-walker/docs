## Setup

Install the [webhook](https://github.com/adnanh/webhook) package
``` bash
sudo apt install webhook
```

When testing run this command to popup an instance hook instance for the `hook.json` config. The `hotreload` flag watches for config file changes and automatically reloads the hook instance.
``` bash
webhook -hooks 'hook.json' -hotreload -verbose
```

Remember, webhook doesn't have a command to create background services.
So, you must either;

- create a specific systemd service for each hook instance
- paste hooks inside `/etc/webhook.conf` and use the existing webhook service


## Webhook Service

Using the webhook service config to run hook instances
``` bash
# paste the hooks in here
sudo nano /etc/webhook.conf

sudo systemctl enable webhook
sudo systemctl start webook
```


## Systemd Service

Using a systemd service to run a hook instance. Give it a name like `webhook.service` and put it inside of `/etc/systemd/system`
``` conf
[Unit]
Description=webhook

[Service]
User=webhook
Group=webhook
ExecStart=webhook -hooks=/path/hook.json -hotreload=false -verbose
WorkingDirectory=/path
KillMode=process
Restart=on-failure

[Install]
WantedBy=multi-user.target
```


Then run and check if it works
``` bash
# reload systemd
sudo systemctl daemon-reload

sudo systemctl enable webhook
sudo systemctl start webhook

# logs
journalctl -u webhook 
```
