# oledb.js

[![npm version](https://img.shields.io/badge/npm-v1.3.0-blue.svg)](https://www.npmjs.com/package/oledb)
[![license](https://img.shields.io/badge/license-MIT-orange.svg)](LICENSE)
[![tips](https://img.shields.io/badge/tips-bitcoin-brightgreen.svg)](https://www.coinbase.com/blahyourhamster)

A small **promise based** module which uses [Edge](https://github.com/tjanczuk/edge) to connect and execute queries for an 
[OLE DB](https://en.wikipedia.org/wiki/OLE_DB), [ODBC](https://en.wikipedia.org/wiki/Open_Database_Connectivity) or [SQL](https://en.wikipedia.org/wiki/SQL) database.

## Example
```
const connectionString = 'provider=vfpoledb;data source=C:/MyDatabase.dbc';

const oledb = require('oledb');
const db = oledb(connectionString);

let command = 'select * from account';

db.query(command)
.then(function(results) {
    console.log(results[0]);
},
function(error) {
    console.error(error);
});
```

## Installation
```
npm install oledb --save
```

## Options
The initializer can take up to two parameters:

- `oledb(connectionString)` - Initializes a connection and assumes you are using an **OLE DB** connection.
- `oledb(connectionString, connectionType)` - Initializes a connection to a database where `connectionType` is either `oledb`, `odbc` or `sql`.

Here is an example:

```
const connectionString = 'Server=myServerAddress;Database=myDataBase;Trusted_Connection=True;';
const connectionType = 'sql';

const oledb = require('oledb');
const db = oledb(connectionString, connectionType);

...
```

## Promises
There are 3 available promises exposed by the module:

- `.query(command, parameters)` - Executes a query and returns the result set returned by the query as an `Array`.
- `.execute(command, parameters)` - Executes a query command and returns the number of rows affected.
- `.scalar(command, parameters)` - Executes a query and returns the first column of the first row in the result set returned by the query. All other columns and rows are ignored.

Where `command` is the query string and `parameters` is an array of parameter values.

*Please note that `parameters` are **optional**.*

## Query Parameters
Parameters are also supported and use positional parameters that are marked with a question mark (?) instead of named parameters. Here is an example:

```
let command = `
    select * from account 
    where
        firstname = ?
        and age = ?
`;

let parameters = [ 'Bob', 69 ];

db.query(command, parameters)
.then(function(results) {
    console.log(results[0]);
},
function(error) {
    console.error(error);
});
```

## Multiple Data Sets
The `.query` promise has support for multiple data sets that can be returned in a single query. Here is an example:

```
let command = `
    select * from account;
    select * from address;
`;

db.query(command)
.then(function(results) {
    console.log(results[0]); //1st data set
    console.log(results[1]); //2nd data set
},
function(error) {
    console.error(error);
});
```

## License
This project is licensed under [MIT](LICENSE).
