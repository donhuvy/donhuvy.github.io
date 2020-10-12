---
layout: post
title:  "Microsoft SQL Server 2019 on Docker"
date:   2020-10-12 21:00:00 +0700
categories: SQLServer Docker
---
Microsoft SQL Server 2019 on Docker

Change Docker data folder (if need): `%APPDATA%\Docker\settings.json`

Change value to

```
"dataFolder": "D:\\ProgramData\\DockerDesktop\\vm-data",
```

Command

```
docker pull mcr.microsoft.com/mssql/server:2019-latest

docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=123456a@' -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

Document: [https://hub.docker.com/_/microsoft-mssql-server](https://hub.docker.com/_/microsoft-mssql-server)
