***Show all databases***

```
mysql> SHOW DATABASES;
```

***Show all users***

```
mysql> SELECT user, host FROM mysql.user;
```

***Create a database***

```
mysql> CREATE DATABASE IF NOT EXISTS database_name;
```

***Create an user***

```
mysql> CREATE USER 'local_user'@'localhost' IDENTIFIED BY 'password';
```

***Grant permissions to a MySQL user account***

Some common privileges include:

`ALL PRIVILEGES`: The user is granted all privileges except GRANT OPTION and PROXY.

`ALTER`: The user can change the structure of a table or database.

`CREATE`: The user can create new databases and tables.

`DELETE`: The user can delete rows in a table.

`INSERT`: The user can add rows to a table.

`SELECT`: The user can read rows from a table.

`UPDATE`: The user can update rows in a table.

```
mysql> GRANT SELECT, INSERT, CREATE, DELETE, UPDATE, ALTER ON database_name.* TO 'local_user'@'localhost';
```

To create a user with the same privileges as the root user, use the following command, which grants global privileges to the user Janet connecting via localhost:

```
mysql> GRANT ALL ON *.* TO 'local_user'@'localhost' WITH GRANT OPTION;
```

The WITH GRANT OPTION clause allows users to grant their privileges to other users.

***Display MySQL user account privileges***

```
mysql> SHOW GRANTS FOR 'local_user'@'localhost';
```

***Revoke permissions from a MySQL user account***

```
mysql> REVOKE SELECT, INSERT ON database_name.* FROM 'local_user'@'localhost';
```

***Delete MySQL users***

```
mysql> DROP USER 'local_user'@'localhost';
```

***Change a MySQL user account password***

The syntax to change a user password depends on your version of MySQL. To find out the MySQL version you are running, use the command:

```
mysql> SELECT version();
```
To change a password for MySQL 5.76 or higher, use this command:

```
mysql> ALTER USER 'local_user'@'localhost' IDENTIFIED BY 'new_password';
```
For older versions of MySQL, use this command instead:

```
mysql> SET PASSWORD FOR 'local_user'@'localhost' = PASSWORD('new_password');
```
