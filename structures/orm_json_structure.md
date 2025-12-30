# ORM JSON

This ORM JSON object represents metadata for an ORM database entity. It defines the table structure, columns, predefined SQL queries (selects), executable operations, triggers (selects/updates/inserts/deletes), authorizations (selects), and referential columns.

The structure is designed for multi-database compatibility, with SQL variants for different engines.

## The JSON

```json
{
  "queryType": "SQL",
  "table": {
    "tableName": "",
    "pkMode": "0",
    "pkSql1": "",
    "pkSql2": "",
    "pkSql3": "",
    "pkSql4": "",
    "insertAuthorization": "",
    "updateAuthorization": "",
    "deleteAuthorization": "",
    "description": "",
    "editorDatabaseID": "",
    "developmentDatabaseID": "",
    "databaseController": "",
    "dbSchema1": "",
    "dbSchema2": "",
    "dbSchema3": "",
    "dbSchema4": ""
  },
  "column": [
    {
      "id": "",
      "columnName": "",
      "type": "",
      "pk": 0,
      "role": "",
      "sqlType1": "",
      "sqlType2": "",
      "sqlType3": "",
      "sqlType4": "",
      "translation": ""
    }
  ],
  "select": [
    {
      "id": "",
      "selectName": "",
      "origin": "",
      "sql1": "",
      "sql2": "",
      "sql3": "",
      "sql4": ""
    }
  ],
  "operation": [
    {
      "id": "",
      "operationName": "",
      "type": "",
      "origin": "",
      "description": "",
      "sql1": "",
      "sql2": "",
      "sql3": "",
      "sql4": ""
    }
  ],
  "authorization": [
    {
      "id": "",
      "authorizationName": "",
      "description": "",
      "sql1": "",
      "sql2": "",
      "sql3": "",
      "sql4": ""
    }
  ],
  "fk": [
    {
      "id": "",
      "columnName": "",
      "type": "",
      "secure": "",
      "role": ""
    }
  ]
}
```


## `queryType` String

Indicates the type of ORM. The value "SQL" indicates the queries are SQL-based. The value "File" means that the queries are file-system-based.

## `table` Object
Uses the following properties.

| Property              | Description                                                  |
| --------------------- | ------------------------------------------------------------ |
| tableName             | Name of the table.                                           |
| pkMode                | Primary key mode: "Auto" - Auto increment by database, "SQL" - SQL Query, "Value" - Value Provided, and "UUID" - Auto-generated UUID. |
| pkSql1                | Used for pkMode=SQL and the database is MS SQL Server.       |
| pkSql2                | Used for pkMode=SQL and the database is MySQL.               |
| pkSql3                | Used for pkMode=SQL and the database is PostgreSQL.          |
| pkSql4                | Used for pkMode=SQL and the database is Oracle.              |
| insertAuthorization   | Authorization for inserts (empty means no authorization required). |
| updateAuthorization   | Authorization for updates (empty means no authorization required). |
| deleteAuthorization   | Authorization for deletes (empty means no authorization required). |
| description           | Table description.                                           |
| editorDatabaseID      | A list of comma-separated database IDs for allowed editors. It should start and end with commas. Example: ",1,2,3,4,". The values allowed in the list are: 1-MS SQL Server, 2-MySQL, 3-PostgreSQL, 4-Oracle. |
| developmentDatabaseID | The development database ID if different from the SilkBuilder installation database. |
| databaseController    | Database controller if different from the default "SilkSqlController". Empty means use the default controller. |
| dbSchema1             | Schema name for MS SQL Server if required.                   |
| dbSchema2             | Schema name for MySQL if needed.                             |
| dbSchema3             | Schema name for PostgreSQL if required.                      |
| dbSchema4             | Schema name for Oracle if needed.                            |

## `column` Array
An array of objects, each describing a column in the table. Each object contains the following attributes.

| Property    | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| id          | Unique column's identifier. Usually a UUID. Only for internal use. |
| columnName  | The column's name.                                           |
| type        | The SilkBuilder column type. Check the list below.           |
| pk          | Indicates if the column is the primary key.                  |
| role        | Specifies the operational role this column represents during ORM operations. Check the List below. |
| sqlType1    | The SQL type if using MS SQL Server.                         |
| sqlType2    | The SQL type if using MySQL.                                 |
| sqlType3    | The SQL type if using PostgreSQL.                            |
| sqlType4    | The SQL type if using Oracle.                                |
| transaction | Set the translation storage type if the column will use it. Check the list below. |

### Type List

* "S" - String
* "I" - Integer
* "N" - Numeric
* "D" - Data/Time
* "P" - Password
* "U" - UUID String automatically added at insert.
* "X" - Public ID. Short table-based unique ID added at insert.

###  Role List

* "X" - Not Applicable, the default values.
* "R" - Tree Root (If different from the Primary Key).
* "P" - Tree Parent
* "L" - Tree Level
* "O" - Row Ordering
* "S" - Operation Status
* "A" - Operation Action
* "D" - Operation Date
* "U" - Operation User

### Translation List

* "0" - None applicable
* "1" - In Column Data
* "2" - By Column

## `select` Array
An array of objects, each describing a Select. Each object contains the following attributes.

| Property    | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| id          | Unique select's identifier. Usually a UUID. Only for internal use. |
| selectName  | Name of the select.                                          |
| origin      | Source of the Select SQL. The default value is "0" (ORM), which retrieves the SQL from the ORM configuration. If the value is set to "1" (Database), the SQL command is extracted via an SQL query from a table. |
| sql1        | The Select SQL if using MS SQL Server.                       |
| sql2        | The Select SQL if using MySQL.                               |
| sql3        | The Select SQL if using PostgreSQL.                          |
| sql4        | The Select SQL if using Oracle.                              |
| description | The select's description                                     |

## `operation` Array
An array of objects, each describing an operation command. Each Object contains the following attributes.

| Property      | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| id            | Unique operation's identifier. Usually a UUID. Only for internal use. |
| type          | Defines how the operation will be executed. If set to "exec," it will be executed using the DataProvider exec method. If set to "trigger," it will be executed during an insert, update, or delete operation. |
| operationName | The name of the operation if the type is set to "exec."      |
| when          | If the operation is set to "trigger," this defines the moment of execution. The values are: "before" or "after". |
| action        | If the operation is set to "trigger," this defines the action triggering the operation. The values are: "insert", "update", or "delete." |
| origin        | Source of the operation SQL. The default value is "0" (ORM), which retrieves the SQL from the ORM configuration. If the value is set to "1" (Database), the SQL command is extracted via an SQL query from a table. |
| description   | The operation's description                                  |
| sql1          | The operation SQL if using MS SQL Server.                    |
| sql2          | The operation SQL if using MySQL                             |
| sql3          | The operation SQL if using PostgreSQL                        |
| sql4          | The operation SQL if using Oracle                            |



## `authorization` Array
An array of objects, each describing an authorization query. Each Object contains the following attributes.

| Property          | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| id                | Unique operation's identifier. Usually a UUID. Only for internal use. |
| authorizationName | The name of this authorization.                              |
| description       | The authorization method.                                    |
| sql1              | The SQL defining the authorization if using MS SQL Server.   |
| sql2              | The SQL defining the authorization if using MySQL.           |
| sql3              | The SQL defining the authorization if using PostgreSQL.      |
| sql4              | The SQL defining the authorization if using Oracle.          |

## `fk` Array
An array of objects, each describing referential columns. These are usually foreign keys from other tables, not related to the current ORM. Each Object contains the following attributes.

| Property   | Description                                                  |
| ---------- | ------------------------------------------------------------ |
| id         | Unique column's identifier. Usually a UUID. Only for internal use. |
| columnName | The name of the column. The same as used in the reference table. |
| type       | The same values as the property "type" in the column's object. |
| secure     | The same values as the property "secure" in the column's object. |
| role       | The same values as the property "role" in the column's object. |

