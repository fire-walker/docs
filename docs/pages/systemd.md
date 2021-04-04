### Service Files

When you need to auto start or keep something running in the background, a systemd service is the best option.

All user created service files should be inside of `etc/systemd/system`. The name you give to the service file, for example `some.service` is what you use with the `systemctl` or `service` command.

For added security, consider creating a [system user](../general/#system-user) for **each** service. Make sure to not reuse an existing one.

Systemd expects the full path of the tool you wish to use. 

Here's a basic systemd service file template.

``` ini
[Unit]
Description=Details about the service

[Service]
User=<user>
Group=<group>
ExecStart=<command-to-start>
WorkingDirectory=/path
KillMode=process
Restart=on-failure ; restarts if it fails

[Install]
WantedBy=multi-user.target
```

``` bash
# reload systemd
sudo systemctl daemon-reload

# start it
sudo systemctl enable <app-service>
sudo systemctl start <app-service>

# status
sudo systemctl status <app-service>

# logs
journalctl -u <app-service> 
```
