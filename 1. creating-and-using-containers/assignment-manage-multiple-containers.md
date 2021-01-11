# Assignment: Manage Multiple Containers

- [x] Run an `nginx`, a `mysql` and a `httpd` server. Run all of them, `--detach`, name them with `--name`.

- [x] `nginx` should listen on `80:80`, `httpd` on `8080:80`, `mysql` on `3306:3306`.
- [x] When running `mysql`, use the `--env` option (or `-e`) to pass in `MYSQL_RANDOM_ROOT_PASSWORD=yes`. Use`docker container logs` on `mysql` to find the random password it created on startup.
- [x] Clean it all up with `docker container stop` and `docker container rm`.
- [x] Use `docker container ls` to ensure everything is correct before and after cleanup.

## Solution

- Run an `nginx`, a `mysql` and a `httpd` server. Run all of them, `--detach`, name them with `--name`.

    ```powershell
    Aditya :: learning-docker » docker container run --publish 81:80 --detach --name nginx_app nginx
    f979d005973b7cfb748280505ce637375235a50d22dbadbd1d63fcca8329eef1
    ```

    Curl the URL http://localhost:81, we get:

    ```html
    <!DOCTYPE html>
    <html>

    <head>
        <title>Welcome to nginx!</title>
        <style>
            body {
                width: 35em;
                margin: 0 auto;
                font-family: Tahoma, Verdana, Arial, sans-serif;
            }
        </style>
    </head>

    <body>
        <h1>Welcome to nginx!</h1>
        <p>If you see this page, the nginx web server is successfully installed and
            working. Further configuration is required.</p>

        <p>For online documentation and support please refer to
            <a href="http://nginx.org/">nginx.org</a>.<br/>
    Commercial support is available at
            <a href="http://nginx.com/">nginx.com</a>.</p>

        <p><em>Thank you for using nginx.</em></p>
    </body>

    </html>
    ```

- When running `mysql`, use the `--env` option (or `-e`) to pass in `MYSQL_RANDOM_ROOT_PASSWORD=yes`. Use`docker container logs` on `mysql` to find the random password it created on startup.

    ```powershell
    Aditya :: learning-docker » docker run --name mysql_app --publish 3306:3306 --env MYSQL_RANDOM_ROOT_PASSWORD=yes --detach mysql
    Unable to find image 'mysql:latest' locally
    latest: Pulling from library/mysql
    6ec7b7d162b2: Already exists
    fedd960d3481: Pull complete
    7ab947313861: Pull complete
    64f92f19e638: Pull complete
    3e80b17bff96: Pull complete
    014e976799f9: Pull complete
    59ae84fee1b3: Pull complete
    ffe10de703ea: Pull complete
    657af6d90c83: Pull complete
    98bfb480322c: Pull complete
    6aa3859c4789: Pull complete
    1ed875d851ef: Pull complete
    Digest: sha256:78800e6d3f1b230e35275145e657b82c3fb02a27b2d8e76aac2f5e90c1c30873
    Status: Downloaded newer image for mysql:latest
    096cb8a5d625b8fb9c39e101f0be8c341f00aa64e5c2f39cdda91e3df84af07c
    ```

    ```
    Aditya :: learning-docker » docker container logs mysql_app
    2021-01-04 02:22:52+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.22-1debian10 started.
    2021-01-04 02:22:52+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
    2021-01-04 02:22:52+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.22-1debian10 started.
    2021-01-04 02:22:52+00:00 [Note] [Entrypoint]: Initializing database files
    2021-01-04T02:22:52.334536Z 0 [System] [MY-013169] [Server] /usr/sbin/mysqld (mysqld 8.0.22) initializing of server in progress as process 44
    2021-01-04T02:22:52.339468Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
    2021-01-04T02:22:54.494983Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
    2021-01-04T02:22:59.201803Z 6 [Warning] [MY-010453] [Server] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
    2021-01-04 02:23:06+00:00 [Note] [Entrypoint]: Database files initialized
    2021-01-04 02:23:06+00:00 [Note] [Entrypoint]: Starting temporary server
    2021-01-04T02:23:07.230132Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.22) starting as process 89
    2021-01-04T02:23:07.256842Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
    2021-01-04T02:23:07.546146Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
    2021-01-04T02:23:07.695290Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Socket: /var/run/mysqld/mysqlx.sock
    2021-01-04T02:23:07.957702Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
    2021-01-04T02:23:07.958738Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
    2021-01-04T02:23:07.972516Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
    2021-01-04T02:23:08.018561Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.22'  socket: '/var/run/mysqld/mysqld.sock'  port: 0  MySQL Community Server - GPL.
    2021-01-04 02:23:08+00:00 [Note] [Entrypoint]: Temporary server started.
    Warning: Unable to load '/usr/share/zoneinfo/iso3166.tab' as time zone. Skipping it.
    Warning: Unable to load '/usr/share/zoneinfo/leap-seconds.list' as time zone. Skipping it.
    Warning: Unable to load '/usr/share/zoneinfo/zone.tab' as time zone. Skipping it.
    Warning: Unable to load '/usr/share/zoneinfo/zone1970.tab' as time zone. Skipping it.
    2021-01-04 02:23:11+00:00 [Note] [Entrypoint]: GENERATED ROOT PASSWORD: Vu6ielah2phohXahmahwoo5xoob0mi7a 👈👈
    
    2021-01-04 02:23:12+00:00 [Note] [Entrypoint]: Stopping temporary server
    2021-01-04T02:23:12.042630Z 10 [System] [MY-013172] [Server] Received SHUTDOWN from user root. Shutting down mysqld (Version: 8.0.22).
    2021-01-04T02:23:15.823039Z 0 [System] [MY-010910] [Server] /usr/sbin/mysqld: Shutdown complete (mysqld 8.0.22)  MySQL Community Server - GPL.
    2021-01-04 02:23:16+00:00 [Note] [Entrypoint]: Temporary server stopped
    
    2021-01-04 02:23:16+00:00 [Note] [Entrypoint]: MySQL init process done. Ready for start up.
    
    2021-01-04T02:23:16.299348Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.22) starting as process 1
    2021-01-04T02:23:16.309609Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
    2021-01-04T02:23:16.531228Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
    2021-01-04T02:23:16.670439Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060, socket: /var/run/mysqld/mysqlx.sock
    2021-01-04T02:23:16.828114Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
    2021-01-04T02:23:16.828334Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
    2021-01-04T02:23:16.833929Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
    2021-01-04T02:23:16.848560Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.22'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server - GPL.
    ```

    Now running `httpd` on `8080:80`.

    ```powershell
    Aditya :: learning-docker » docker container run --publish 8080:80 --detach --name httpd_app httpd
    0c4fe679654b211950e180bae7925c673fbc99da735809b03c371ddef685473e
    ```

    Curl this URL http://localhost:8080.

    ```html
    <html>
    
    <body>
    	<h1>It works!</h1>
    </body>
    
    </html>
    ```

- Clean it all up with `docker container stop` and `docker container rm`. Use `docker container ls` to ensure everything is correct before and after cleanup.

    ```powershell
    Aditya :: learning-docker » docker container ls -a
    CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                  NAMES
    0c4fe679654b   httpd     "httpd-foreground"       3 minutes ago    Up 3 minutes    0.0.0.0:8080->80/tcp   httpd_app
    096cb8a5d625   mysql     "docker-entrypoint.s…"   12 minutes ago   Up 12 minutes   3306/tcp, 33060/tcp    mysql_app
    f979d005973b   nginx     "/docker-entrypoint.…"   16 minutes ago   Up 16 minutes   0.0.0.0:81->80/tcp     nginx_app
    ```

    ```powershell
    Aditya :: learning-docker » docker container rm -f httpd_app mysql_app nginx_app
    httpd_app
    mysql_app
    nginx_app
    ```

    ```powershell
    Aditya :: learning-docker » docker container ls -a
    CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
    ```

    

