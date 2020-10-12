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
```
