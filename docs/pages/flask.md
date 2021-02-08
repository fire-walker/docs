## No Cache

Flask by default caches files on client devices. When in development, this is a goddamn headache. Especially when you're writing CSS. Unlike VSCode's `live reload`, syncing stylesheet and script changes in a flask app requires you to fully reload a page. This is done by a hard reload using ++ctrl+f5++. The code below counters this by specifing in the response header telling clients not to save anything.
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


## Connecting a DB

First install and configure SQL. Find out how [here](../SQL). Then create a seperate database and user for the web-app.
``` sql
-- create db
CREATE DATABASE <app-db-name>;

-- create user
CREATE USER '<username>'@localhost IDENTIFIED BY '<password>';
```


Give the `<username>` user access to the `<app-db-name>` db[^1].
``` sql
-- check user
SELECT User FROM mysql.user;

-- grant privileges
GRANT ALL PRIVILEGES ON '<app-db-name>'.* TO '<username>'@localhost;

-- save grant changes
FLUSH PRIVILEGES;

-- check privileges
SHOW GRANTS FOR '<username>'@localhost;
```

Reference the db from the flask app. To get MariaDB working on SQLAlchemy you'll also need to install the `pymysql` package[^2].
``` py3
app.config['SQLALCHEMY_DATABASE_URI'] = "mysql+pymysql://<username>:<password>@localhost/<app-db-name>?charset=utf8mb4"
```









[^1]: https://phoenixnap.com/kb/how-to-create-mariadb-user-grant-privileges
[^2]: https://stackoverflow.com/questions/54834088/python-database-connection-for-mariadb-using-sqlalchemy