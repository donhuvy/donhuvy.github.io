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

docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=123456a@" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest

docker logs -f 1b2ce04432883df593535157999ae3d2996620155b71e0e1d9fd25e8b2e60628
```

Document: [https://hub.docker.com/_/microsoft-mssql-server](https://hub.docker.com/_/microsoft-mssql-server)


Example: Use NodeJS 14.13.11 , NPM 6.14.8 (latest tools at Oct 12th, 2020)

File `index.js`

```js
const sql = require('mssql');

const config = {
    user: 'sa',
    password: '123456a@',
    server: 'localhost',
    database: 'testdb',
    options: {
        enableArithAbort: true
    }
}

const run = async () => {
    let pool;
    try {
        console.log('Connection Opening...');
        pool = await sql.connect(config);
        const { recordset } = await sql.query`select * from users;`;

        console.log(recordset);
    } catch (err) {
        console.log(err);
    } finally {
        await pool.close();
        console.log('Connection closed');
    }
}

run();

// NodeJS 14.13.11
// npm 6.14.8
// npm install --save mssql

// https://www.youtube.com/watch?v=RAE-VcZ3u2A
// docker-compose up -d
// docker-compose ps
// docker logs sql-server-db

// docker exec -it sql-server-db "bash"
// /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 123456a@

// docker-compose down
// select name from sys.Databases;
// go

// create database testdb;
// go

// select name form sys.Databases;
// go

// use testdb
// go

// create table users (id INT, name NVARCHAR(50), email NVARCHAR(255));
// go

// insert into users values(1, 'Bill Gates', 'bill.gates@microsoft.com');
// go

// select * from users;
// go

// Ctrl + C, exit
```

File `docker-compose.yml`

```yml
version: "3.7"
services: 
    sql-server-db:
        container_name: sql-server-db
        image: mcr.microsoft.com/mssql/server:2019-latest
        ports: 
            - "1433:1433"
        environment: 
            SA_PASSWORD: "123456a@"
            ACCEPT_EULA: "Y"
```
Can connect from SQL Server Management Studio success.

Run command to see result

```
node ./index.js
```
