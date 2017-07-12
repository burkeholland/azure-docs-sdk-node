---
title: Azure SQL modules for Node.js
description: Connect and manage Azure SQL from your Node.js apps.
keywords: Azure, Node, SDK, API, nodejs, javascript, sql
author: tomarcher
ms.author: tarcher
manager: douge
ms.date: 06/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: nodejs
ms.service: sql-database
---

# Azure SQL packages for Node.js

## Overview

Work with data stored in [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview) from Node.js.
The management library provides an interface to make it easy to manage Microsoft Azure SQL databases.

## Client Package

#### Install packages with npm

Use npm to install the SQL Server client module.

```bash
npm install tedious
```

### Example

Connect to a SQL Server database and perform a simple query.

```javascript
const Connection = require('tedious').Connection;
const Request = require('tedious').Request;

const config = {
  userName: 'your-username',
  password: 'your-password',
  server: 'path-to-server',
  options: {
    database: 'database-name',
    encrypt: true
  }
};

const connection = new Connection(config);
connection.on('connect', err => {
  err ? console.log(err) : executeStatement();
});

const query = 'SELECT * from TableName';
const executeStatement = () => {
  const request = new Request(query, (err, rowCount) => {
    err ? console.log(err) : console.log(rowCount);
  });

  request.on('row', columns => {
    columns.forEach(column => console.log(column.value));
  });

  connection.execSql(request);
};
```

## Management Package

#### Install packages with npm

Use npm to install the Azure SQL Server management module.

```
npm install azure-arm-sql
```   

### Example

Authenticate, create a client, and list all servers.

```javascript
const msRestAzure = require('ms-rest-azure');
const SQLManagement = require('azure-arm-sql');

msRestAzure
  .interactiveLogin()
  .then(credentials => {
    const client = new SQLManagement(credentials, 'your-subscription-id');
    return client.servers.list();
  })
  .then(servers => console.dir(servers, { depth: null, colors: true }))
  .catch(err => console.log(err));
```

## Samples

[!INCLUDE [node-sql-samples](../docs-ref-conceptual/includes/sql-samples.md)]

Explore more [sample Node.js code](https://azure.microsoft.com/resources/samples/?platform=nodejs) you can use in your apps.
