# Setup Mysql

## Setup local mysql from Zip distro

* Download Zip Archive (Community Edition)
* Unzip the archive and go to the bin folder.
* Initialize MySql database: Run the following on Command Prompt (cmd) while being in the bin folder. This creates data folder with database files.

   `mysqld --initialize`
   
* Start server with unrestricted access to the full database.

    `mysqld --console --skip-grant-tables --shared-memory`
    
* Reset password for root user

 
    mysql> FLUSH PRIVILEGES;
    
    mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
    
* Restart server in normal mode.

    `mysqld --console`
