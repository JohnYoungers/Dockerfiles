This is an extension of `microsoft/mssql-server-linux` with `openjdk-8-jre` and `flyway` installed.

Sample usage:

```Dockerfile
FROM youngersconsulting/mssql-server-linux-flyway
ENV ACCEPT_EULA Y
ENV SA_PASSWORD sa_password!123456

COPY ./entrypoint.sh .
COPY ./dbsetup.sh .
COPY ./MyDb.bak .
COPY ./Migrations /flyway/sql

CMD [ "/bin/bash", "./entrypoint.sh"]
```

entrypoint.sh:

```
/dbsetup.sh & /opt/mssql/bin/sqlservr
```

dbsetup.sh (1 - restores from a backup file, 2 - executes sql scripts, 3 - executes flyway migrate)

```
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "sa_password!123456" -Q "RESTORE DATABASE MyDb FROM DISK = '/MyDb.bak' WITH MOVE 'MyDb' TO '/var/opt/mssql/data/MyDb.mdf', MOVE 'MyDb_log' TO '/var/opt/mssql/data/MyDb_log.ldf'"
find . -type f -name "*.sql" -exec /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "sa_password!123456" -d MyDb -i "{}" \;
flyway/flyway -url="jdbc:sqlserver://localhost:1433;databaseName=MyDb" -baselineOnMigrate=true -user="sa" -password="sa_password!123456" migrate
```

Running your dockerfile:

```
docker build -t MyDb_sql .
docker run --name MyDb_sql -p 1401:1433 -d MyDb_sql
```
