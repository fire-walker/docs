## Setup

Install the [webhook](https://github.com/adnanh/webhook) package

``` bash
sudo apt install webhook
```

Create a system user and group to handle webhook instance services. Check out how [here](../general/#system-user).

Then go ahead and create a `<hookname>.json` file. Check out the official [docs](https://github.com/adnanh/webhook/tree/master/docs) for more info.

## Testing

When testing run this command to pop a hook instance for the `hook.json` config. The `hotreload` flag watches for config file changes and automatically reloads the hook instance.

``` bash
webhook -hooks 'hook.json' -hotreload -verbose
```

The above will start up on the default port `9000`. The `hook-name` is what's specified in the `id` field of the json config file.

``` link
http://localhost:9000/hooks/hook-name
```

## Connections

### Github

Check out how [here](https://docs.github.com/en/github-ae@latest/developers/webhooks-and-events/creating-webhooks)

## Instance Service

Remember, webhook doesn't have a command to create background services.
So, you must either;

!!! info ""
    Create a specific systemd service for each hook instance

!!! info ""
    Paste hooks inside `/etc/webhook.conf` and use the existing `webhook` service

Then route all traffic from the port through an Nginx reverse proxy using an SSL cert provided by certbot and your done.

### Existing Service

Using the default webhook service config to run hook instances. 

If `/etc/webhook.conf` doesn't exist, just create a new one. The service file named `webhook.service` should exist at `/etc/systemd/system` by default. If not go to the next topic and manually create one.

``` bash
# paste the hooks in here
sudo nano /etc/webhook.conf

# start it
sudo systemctl enable webhook
sudo systemctl start webhook
```

### Systemd Service

Using a systemd service to run a hook instance. 

Give it a name like `webhook-one.service` and put it inside of `/etc/systemd/system`. Make sure to intialize and reload the service and systemd. Creating a seperate [group](../general/#groups) and [system user](../general/#system-user) is highly recommended.

``` ini
[Unit]
Description=webhook-one

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
