# PSQL commands 

### Connect to PostgreSQL database
The following command connect to a database under a specific *user*. After pressing **Enter** PostgreSQL ask for the password of the *user*.

``` shell
psql - d database -U user 
```

### Display databases
To list all databases in the current PostgreSQL databases server, you use `\l` command

``` shell 
\l
```
### Switch connection to a new database
Once you are connected to a database, you can switch the connection to a new database under a user specified by *user*. The previous connection will be closed. If you the *user* parameter, the current *user* is assumend.

```shell
\c dbname username 
``` 

### List available tables

To list all tables in the current database, you use `\dt` command:

```shell
\dt 
```

### Describe a table 
To describe a table such as a column, type, modifiers of columns, etc., you use the following command: 

``` shell
\d table_name
```