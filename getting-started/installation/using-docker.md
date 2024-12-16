---
description: >-
  Speedtest Tracker can be run on a variety of platforms, the preferred platform
  is  Docker.
---

# Using Docker or Docker Compose

Setting up your environment with Docker Compose is the recommended way as it'll setup the application and a database for you. These steps will run you through setting up the application using Docker and Docker Compose.

### Install with Docker

{% hint style="info" %}
The Docker Run commands can be found below the compose examples. These instructions assume you have an appropriate database instance that already exists.
{% endhint %}

{% stepper %}
{% step %}
**Docker Compose**

SQLite is fine for most installs but we also provide instructions for setting up MariaDB, MySQL and Postgres should you prefer those database drivers.

{% hint style="info" %}
If you would like to provide your own SSL keys, they must be named `cert.crt` (full chain) and `cert.key` (private key), and mounted in the container folder `/config/keys`.
{% endhint %}

{% tabs %}
{% tab title="SQLite" %}
```yaml
services:
    speedtest-tracker:
        image: lscr.io/linuxserver/speedtest-tracker:latest
        restart: unless-stopped
        container_name: speedtest-tracker
        ports:
            - 8080:80
            - 8443:443
        environment:
            - PUID=1000
            - PGID=1000
            - DB_CONNECTION=sqlite
            - APP_KEY=
            - DATETIME_FORMAT=
            - APP_TIMEZONE=
            - SPEEDTEST_SCHEDULE= # Optional
            - SPEEDTEST_SERVERS= # Optional
        volumes:
            - /path/to/data:/config
            - /path/to-custom-ssl-keys:/config/keys
```

***

```
docker run -d --name speedtest-tracker --restart unless-stopped \
    -p 8080:80 \
    -p 8443:443 \
    -e PUID=1000 \
    -e PGID=1000 \
    -e DB_CONNECTION=sqlite \
    -e APP_KEY= \
    -e DATETIME_FORMAT= \
    -e APP_TIMEZONE= \
    -e SPEEDTEST_SCHEDULE= \
    -e SPEEDTEST_SERVERS= \
    -v /path/to/data:/config \
    -v /path/to-custom-ssl-keys:/config/keys \
    lscr.io/linuxserver/speedtest-tracker:latest
```
{% endtab %}

{% tab title="MariaDB" %}
<pre class="language-yaml"><code class="lang-yaml">services:
<strong>    speedtest-tracker:
</strong>        image: lscr.io/linuxserver/speedtest-tracker:latest
        restart: unless-stopped
        container_name: speedtest-tracker
        ports:
            - 8080:80
            - 8443:443
        environment:
            - PUID=1000
            - PGID=1000
            - DB_CONNECTION=mariadb
            - DB_HOST=db
            - DB_PORT=3306
            - DB_DATABASE=speedtest_tracker
            - DB_USERNAME=speedy
            - DB_PASSWORD=password
            - APP_KEY=
            - DATETIME_FORMAT=
            - APP_TIMEZONE=
            - SPEEDTEST_SCHEDULE= # Optional
            - SPEEDTEST_SERVERS= # Optional
        volumes:
            - /path/to/data:/config
            - /path/to-custom-ssl-keys:/config/keys
        depends_on:
            - db
    db:
        image: mariadb:10
        restart: always
        environment:
            - MARIADB_DATABASE=speedtest_tracker
            - MARIADB_USER=speedy
            - MARIADB_PASSWORD=password
            - MARIADB_RANDOM_ROOT_PASSWORD=true
        volumes:
            - speedtest-db:/var/lib/mysql
volumes:
  speedtest-db:
</code></pre>

***

```
docker run -d --name speedtest-tracker --restart unless-stopped \
    -p 8080:80 \
    -p 8443:443 \
    -e PUID=1000 \
    -e PGID=1000 \
    -e DB_CONNECTION=mariadb \
    -e DB_HOST= \
    -e DB_PORT=3306 \
    -e DB_DATABASE=speedtest_tracker \
    -e DB_USERNAME= \
    -e DB_PASSWORD= \
    -e APP_KEY= \
    -e DATETIME_FORMAT= \
    -e APP_TIMEZONE= \
    -e SPEEDTEST_SCHEDULE= \
    -e SPEEDTEST_SERVERS= \
    -v /path/to/data:/config \
    -v /path/to-custom-ssl-keys:/config/keys \
    lscr.io/linuxserver/speedtest-tracker:latest
```
{% endtab %}

{% tab title="MySQL" %}
```yaml
services:
    speedtest-tracker:
        image: lscr.io/linuxserver/speedtest-tracker:latest
        restart: unless-stopped
        container_name: speedtest-tracker
        ports:
            - 8080:80
            - 8443:443
        environment:
            - PUID=1000
            - PGID=1000
            - DB_CONNECTION=mysql
            - DB_HOST=db
            - DB_PORT=3306
            - DB_DATABASE=speedtest_tracker
            - DB_USERNAME=speedy
            - DB_PASSWORD=password
            - APP_KEY=
            - DATETIME_FORMAT=
            - APP_TIMEZONE=
            - SPEEDTEST_SCHEDULE= # Optional
            - SPEEDTEST_SERVERS= # Optional
        volumes:
            - /path/to/data:/config
            - /path/to-custom-ssl-keys:/config/keys
        depends_on:
            - db
    db:
        image: mysql:9
        restart: always
        environment:
            - MARIADB_DATABASE=speedtest_tracker
            - MARIADB_USER=speedy
            - MARIADB_PASSWORD=password
            - MARIADB_RANDOM_ROOT_PASSWORD=true
        volumes:
            - speedtest-db:/var/lib/mysql
volumes:
  speedtest-db:
```

***

```
docker run -d --name speedtest-tracker --restart unless-stopped \   
    -p 8080:80 \
    -p 8443:443 \
    -e PUID=1000 \
    -e PGID=1000 \
    -e DB_CONNECTION=mysql \
    -e DB_HOST= \
    -e DB_PORT=3306 \
    -e DB_DATABASE=speedtest_tracker \
    -e DB_USERNAME= \
    -e DB_PASSWORD= \
    -e APP_KEY= \
    -e DATETIME_FORMAT= \
    -e APP_TIMEZONE= \
    -e SPEEDTEST_SCHEDULE= \
    -e SPEEDTEST_SERVERS= \
    -v /path/to/data:/config \
    -v /path/to-custom-ssl-keys:/config/keys \
    lscr.io/linuxserver/speedtest-tracker:latest
```
{% endtab %}

{% tab title="Postgres" %}
```yaml
services:
    speedtest-tracker:
        image: lscr.io/linuxserver/speedtest-tracker:latest
        restart: unless-stopped
        container_name: speedtest-tracker
        ports:
            - 8080:80
            - 8443:443
        environment:
            - PUID=1000
            - PGID=1000
            - DB_CONNECTION=pgsql
            - DB_HOST=db
            - DB_PORT=5432
            - DB_DATABASE=speedtest_tracker
            - DB_USERNAME=speedy
            - DB_PASSWORD=password
            - APP_KEY=
            - DATETIME_FORMAT=
            - APP_TIMEZONE=
            - SPEEDTEST_SCHEDULE= # Optional
            - SPEEDTEST_SERVERS= # Optional
        volumes:
            - /path/to/data:/config
            - /path/to-custom-ssl-keys:/config/keys
        depends_on:
            - db
    db:
        image: postgres:17
        restart: always
        environment:
            - POSTGRES_DB=speedtest_tracker
            - POSTGRES_USER=speedy
            - POSTGRES_PASSWORD=password
        volumes:
            - speedtest-db:/var/lib/postgresql/data
volumes:
  speedtest-db:
```

***

```
docker run -d --name speedtest-tracker --restart unless-stopped \   
    -p 8080:80 \
    -p 8443:443 \
    -e PUID=1000 \
    -e PGID=1000 \
    -e DB_CONNECTION=pgsql \
    -e DB_HOST= \
    -e DB_PORT=5432 \
    -e DB_DATABASE=speedtest_tracker \
    -e DB_USERNAME= \
    -e DB_PASSWORD= \
    -e APP_KEY= \
    -e DATETIME_FORMAT= \
    -e APP_TIMEZONE= \
    -e SPEEDTEST_SCHEDULE= \
    -e SPEEDTEST_SERVERS= \
    -v /path/to/data:/config \
    -v /path/to-custom-ssl-keys:/config/keys \
    lscr.io/linuxserver/speedtest-tracker:latest
```
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
**Environment Variables**

In order for the application to run smoothly, some environment variables need to be set. Check out the [Environment Variables](../environment-variables.md) section. Make sure you set all the **Required** variables.
{% endstep %}

{% step %}
**Speedtest Variables**

Optionally you can set variables to have automatic speedtest on an schedule. Check out the [Environment Variables](../environment-variables.md#speedtest) section on how to set the variables. As well see the [FAQ](../../help/faqs.md#speedtest) for tips on the best schedule
{% endstep %}

{% step %}
**Start the Container**

You can now start the container accordingly the platform you are on.
{% endstep %}

{% step %}
**First Login**

During the start the container there is an default username and password created. Use the [default login](../../security/authentication.md#default-user-account) credentials to login to the application. You can [change the default user](../../security/authentication.md#change-account-details) after logging in.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Complete overview of the Environment Variables for custom configuration can be found [here](../environment-variables.md)
{% endhint %}