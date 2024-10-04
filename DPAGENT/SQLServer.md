# Setup SQL Server and connectivity for DP Agent

I am creating a Docker environment in OPenSuse Tumbleweed, and running SQL Server 2019 (15) from there.

NB! ODBC Driver 18 has breaking changes - encryption is by default on, and it is checking against certificates, which are self-signed

Resources:
* [Quickstart: Run SQL Server Linux container images with Docker](https://learn.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-ver15&tabs=cli&pivots=cs1-bash)
* [Docker install - Tumbleweed](https://en.opensuse.org/Docker)
* [sqlcmd utility](https://learn.microsoft.com/en-us/sql/tools/sqlcmd/sqlcmd-utility?view=sql-server-ver16&tabs=go%2Cwindows&pivots=cs1-bash)

```console
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<some_password_here>" \
    -p 1433:1433 --name sql1 --hostname sql1 \
    -d \
    mcr.microsoft.com/mssql/server:2019-latest
```

```console
docker exec -it sql1 "bash"
```

Using `-C` to trust self-signed certificate
```console
mssql@sql1:/$ /opt/mssql-tools18/bin/sqlcmd -S localhost -U sa -P <some_password_here> -C
```

From inside container - to install Pubs database.
```console
cd /var/opt/mssql
wget https://github.com/microsoft/sql-server-samples/raw/refs/heads/master/samples/databases/northwind-pubs/instpubs.sql
/opt/mssql-tools18/bin/sqlcmd -S localhost -U sa -P <some_password_here> -C -i instpubs.sql
```

