# How to install LAMP stack on CentOS 7

#### Install Apache2

1. Install Apache2

    Apache is available in the default CentOS 7 repositories and the installation is pretty straight forward. To install the package run the following command

    `yum install httpd`
    
    Once the installation is completed, start and enable the Apache service by typing
    
    ```
   systemctl start httpd
   systemctl enable httpd
   ```
   
#### Install MySQL

1. Enable the MySQL 5.7/8.0 repository with the following command

   ```
   yum localinstall https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
   yum localinstall https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm
   ```
    
2. Install MySQL package with yum 
    
    `yum install mysql-community-server`
    
3. Once the installation is completed, start the MySQL service and enable it to automatically start on boot with

    ```
    systemctl enable mysqld
    systemctl start mysqld
    ```
4. Securing MySQL

    When the MySQL server is started for the first time, a temporary password is generated for the MySQL root user. You can find the password by running the following command
    
    `grep 'temporary password' /var/log/mysqld.log`
    
    Make note of the password, because the next command will ask you to enter the temporary root password
    
    Run the `mysql_secure_installation` command to improve the security of our MySQL installation
    
    `mysql_secure_installation`
    
    The output should look something like this
    
    ```
   Securing the MySQL server deployment.
   Enter password for user root:
   ```
   
   After entering the temporary password you will be asked to set a new password for user root. The password needs to
    be **at least 8-characters long and to contain at least one uppercase letter, one lowercase letter, one number, and one special character**.
    
    ```
   The existing password for the user account root has expired. Please set a new password.
   New password:
   Re-enter new password:
   ```
   
   Then the script will also ask you to remove the anonymous user, restrict root user access to the local machine and
    remove the test database. You should **answer “Y” (yes) to all questions**.

#### Install PHP7

1. Install EPEL repo

    `yum install epel-release`

2. Install Remi repo

    `yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm`

3. Install **yum-utils** - _a collection of useful programs for managing yum repositories and packages. It can be used
 for managing (enabling or disabling) yum repositories as well as packages without any manual configuration and so much more_.
 
    `yum install yum-utils`
 
 4. Enable Remi repository as the default repository for installing different PHP versions
 
    ```
    yum-config-manager --enable remi-php71
    yum-config-manager --enable remi-php72
    yum-config-manager --enable remi-php73
    ```
 
 5. Now we can install several most common PHP modules
 
    ```
    yum install php php-common php-devel php-opcache php-cli php-mcrypt php-mbstring php-bcmath php-json php-mysqlnd
     php-event php-zip php-xml php-xmlrpc
    ```
    
 #### Install `libsodium`
 
 1. Install both the library and PHP extension
  
   ```
    yum install libsodium-devel
    yum install php-sodium
   ```
    
 2. Verify that you have the correct version of `libsodium` installed
 
   
   ```
    <?php
    var_dump([
        SODIUM_LIBRARY_MAJOR_VERSION,
        SODIUM_LIBRARY_MINOR_VERSION,
        SODIUM_LIBRARY_VERSION
    ]);
  ```
  