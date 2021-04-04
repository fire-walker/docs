## No Cache

Flask by default caches files on client devices. When in development, this is a goddamn headache. Especially when you're writing CSS. Unlike VSCode's `live reload`, syncing stylesheet and script changes in a flask app requires you to fully reload a page. This is done by a hard reload using ++ctrl+f5++. The code below counters this by specifying in the response header telling clients not to save anything.

``` py3
# don't fucking save assets in cache
@app.after_request
def add_header(r):
    r.headers["Cache-Control"] = "no-cache, no-store, must-revalidate"
    r.headers["Pragma"] = "no-cache"
    r.headers["Expires"] = "0"
    r.headers["Cache-Control"] = "public, max-age=0"
    return r
```

## Deployment

- [x] Setup database
- [x] Create systemd service for gunicorn
- [x] Create hook instance config json
- [x] Configure GitHub repo's webhook settings
- [x] Create a group for the hook service
- [x] Create a systemd service for the hook instance
- [x] Write the pull and reload script

## Connecting a DB

First create a separate database and user for the web-app in the [usual way](../database#usual-process). Then reference the db from the flask app. To get MariaDB working on SQLAlchemy you'll also need to install the `pymysql` package[^2].

``` py3
app.config['SQLALCHEMY_DATABASE_URI'] = "mysql+pymysql://<username>:<password>@localhost/<app-db-name>?charset=utf8mb4"
```

## Gunicorn Service

To run a flask app in development you need a WSGI HTTP Server to serve and run it. This is where `gunicorn` comes in. 

### Development

When in development you can run `gunicorn` in the shell. The `<app-name>` refers to the file name of the flask app and the `app` refers to the name of the callable within it.

``` bash
# run it on port 5000
gunicorn --bind localhost:5000 <app-name>:app
```

### Production

When deploying, create a systemd service for the `gunicorn`. See more info about creating systemd services [here](../systemd/#service-files). 

Find info about workers and optimizations [here](https://medium.com/building-the-system/gunicorn-3-means-of-concurrency-efbb547674b7).

Here's an example systemd config for gunicorn.

``` ini
[Unit]
Description=Gunicorn instance serving the app
After=network.target

[Service]
User=<reg_user>
Group=www-data
WorkingDirectory=/home/<reg_user>/project
ExecStart=/usr/local/bin/gunicorn --workers 3 --bind localhost:5000 <app-name>:app

[Install]
WantedBy=multi-user.target
```

## Hook Instance

Create a webhook instance json file and connect it with Github to listen to push requests from the repo. More info [here](../webhooks/#setup).

Then create a [new group](../general/#groups) for the flask app and couple it with the already existing `webhook` user created when setting up the webhook application. In it [whitelist](../general/#whitelisting) the `systemctl` commands for dealing with the gunicorn service.

Write a systemd service file with the `webhook` user and the new group for the hook instance. Find out how [here](../webhooks/#systemd-service).

## Integration Script

Since systemd runs services in isolated environments. Without access to the user shell nor any environment variables. So, for private repos an ssh-agent needs to be started every time the script is run for Github authentication. More info [here](../git/#automated-deployment).

First clean the repo of any untracked changes. Then pull the latest commit. Find how [here](). Finally reload the site. Make sure to only use the `systemctl` commands that were defined in the group the hook instance is running on. 

``` bash
systemctl restart <app-service>
```

[^1]: https://phoenixnap.com/kb/how-to-create-mariadb-user-grant-privileges
[^2]: https://stackoverflow.com/questions/54834088/python-database-connection-for-mariadb-using-sqlalchemy
