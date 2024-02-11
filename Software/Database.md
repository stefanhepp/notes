MariaDB
-------

- Set root password
  ```
  # Login to mysql with no password
  mysql -u root -p
  ```
  Use the following to set root password
  ```
  ALTER USER 'root'@'localhost' IDENTIFIED BY 'newpassword';
  FLUSH PRIVILEGES;
  ```
  Don't forget to clear your history!
  ```
  rm -f ~/.mysql_history
  ```
- Grant read access to a user
  ```
  CREATE USER 'backupuser'@'%' IDENTIFIED BY 'newpassword';
  GRANT SELECT, SHOW VIEW ON *.* TO 'backupuser'@'%';
  FLUSH PRIVILEGES;
  ```
- Privileges for backup user:
  - Grant SELECT, SHOW VIEW privileges
  - Use --single-transaction for mysqldump to disable LOCK TABLES
- Restore SQL dump:
  ```
  # mysql -u root -p
  CREATE DATABASE mydatabase;
  USE mydatabase;
  SOURCE path/to/mydatabase.sql;
  ```
- Rename user or change host of user:
  ```
  RENAME USER 'user'@'oldhost' TO 'user'@'newhost';
  ```

