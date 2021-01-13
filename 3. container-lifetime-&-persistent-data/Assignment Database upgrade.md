# Assignment: Named Volumes

- [x] Create a `postgres` container with named volume `psql-data` using `9.6.1`.

- [x] Use Docker Hub to learn `VOLUME` path and versions needed to run it.

- [x] Check logs. stop container

- [x] Create a `postgres` container with same named volume using `9.6.2`.

- [x] Check logs to validate.

  > This only works with patch versions, most SQ DB's require manual commands to upgrade DB's to major/minor versions, i.e., it's a DB limitation not a container one.

## Solution:

Checking the `VOLUME` path in Dockerfile for `Postgres` image.

```
VOLUME [/var/lib/postgresql/data]
```

Creating a named volume `psql-data`.

```powershell
Aditya :: System32 » docker volume create psql-data
psql-data
Aditya :: System32 » docker volume ls
DRIVER    VOLUME NAME
local     psql-data
```

Now creating the first container using `postgres:9.6.1` image.

```powershell
Aditya :: System32 » docker container run --name psql1 -v psql-data:/var/lib/postgresql/data -d -e POSTGRES_PASSWORD=secret postgres:9.6.1
a71da876eb228b0762b6eae83ddcc033b01c7a40ece794231a05a4236d882089
```

Now, checking the logs from the container `psql1`.

```powershell
Aditya :: System32 » docker container logs psql1
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.utf8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /var/lib/postgresql/data ... ok
creating subdirectories ... ok
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting dynamic shared memory implementation ... posix
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok

WARNING: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.
syncing data to disk ... ok

Success. You can now start the database server using:

    pg_ctl -D /var/lib/postgresql/data -l logfile start

waiting for server to start....LOG:  could not bind IPv6 socket: Cannot assign requested address
HINT:  Is another postmaster already running on port 5432? If not, wait a few seconds and retry.
LOG:  database system was shut down at 2021-01-13 14:47:12 UTC
LOG:  MultiXact member wraparound protections are now enabled
LOG:  database system is ready to accept connections
LOG:  autovacuum launcher started
 done
server started
ALTER ROLE


/docker-entrypoint.sh: ignoring /docker-entrypoint-initdb.d/*

waiting for server to shut down...LOG:  received fast shutdown request
.LOG:  aborting any active transactions
LOG:  autovacuum launcher shutting down
LOG:  shutting down
LOG:  database system is shut down
 done
server stopped

PostgreSQL init process complete; ready for start up.

LOG:  database system was shut down at 2021-01-13 14:47:13 UTC
LOG:  MultiXact member wraparound protections are now enabled
LOG:  database system is ready to accept connections
LOG:  autovacuum launcher started
```

Now, stopping `psql1` and creating the next container `psql2` under the image `postgres:9.6.2`.

```powershell
Aditya :: System32 » docker container rm -f psql1
psql1
Aditya :: System32 » docker container run --name psql2 -v psql-data:/var/lib/postgresql/data -d -e POSTGRES_PASSWORD=secret postgres:9.6.1
5abb9e233ff61858274e046e6998190a8a1f69d8eb7dafc66713792aeb22d691
Aditya :: System32 » docker container logs psql2
LOG:  database system was interrupted; last known up at 2021-01-13 14:47:20 UTC
LOG:  database system was not properly shut down; automatic recovery in progress
LOG:  invalid record length at 0/14EE2C8: wanted 24, got 0
LOG:  redo is not required
LOG:  MultiXact member wraparound protections are now enabled
LOG:  database system is ready to accept connections
LOG:  autovacuum launcher started
```

