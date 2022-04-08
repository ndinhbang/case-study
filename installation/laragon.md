Install mysql 8.0 in laragon

1/ Delete largon/data/mysql-8

2/ cd to laragon/bin/mysql8.0.0.x/bin folder via cmder (important for step 3)

3/ run 'mysqld --initialize --console' (remember the password)

4/ change mysql version to 8.0, then start laragon

5/ run 'mysql -u root -p' via cmder (password was printed on step 3)

6/ You are now in mysql shell, change root password with command: 

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
