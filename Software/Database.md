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

