# How to install and secure `phpmyadmin` with apache on CentOS 7

_After LAMP stack already installed, we can begin right away with installing the phpMyAdmin_

1. Install EPEL repo (**E**xtra **P**ackages for **E**nterprise **L**inux)

    `yum install epel-release`
    
2. Install the phpMyAdmin package

    `yum install phpmyadmin`
    
3. Open phpMyAdmin Apache configuration file 

    `vi /etc/httpd/conf.d/phpMyAdmin.conf`
    
5. Restart the Apache web server to implement your modifications

    `systemctl restart httpd.service`
    
   To access the interface, go to your server’s domain name or public IP address followed by `/phpMyAdmin` 
   , in your web browser
   
   `http://server_domain_or_IP/phpMyAdmin`
   
6. To change the URL where our phpMyAdmin interface can be accessed, we simply need to rename the alias. Open the
 phpMyAdmin Apache configuration file now
 
    `vi /etc/httpd/conf.d/phpMyAdmin.conf`
    
    comment out the existing lines and add our own alias
    
    ```
    # Alias /phpMyAdmin /usr/share/phpMyAdmin
    # Alias /phpmyadmin /usr/share/phpMyAdmin
    Alias /nothingtosee /usr/share/phpMyAdmin
    ...
    ```
   To implement the changes, restart the web service
   
   `systemctl restart httpd.service`
   
   Your phpMyAdmin interface will now be available at the new location
   
   `http://server_domain_or_IP/nothingtosee`
   
7. Setting up a Web Server Authentication Gate

    Open the phpMyAdmin Apache configuration file in your text editor again
    
    `vi /etc/httpd/conf.d/phpMyAdmin.conf`
    
    add an override directive. It will look like below
    
    ```
   ...
   <Directory /usr/share/phpMyAdmin/>
      AllowOverride All
      <IfModule mod_authz_core.c>
      ...
   </Directory>
   ...
    ```
   Restart the web service to implement this change:
   
   `systemctl restart httpd.service`
   
   Now that we have the override directive in our configuration, Apache will look for a file called `.htaccess` within the `/usr/share/phpMyAdmin` directory. If it finds one, it will use the directives contained within to supplement its previous configuration data.
   
   Our next step is to create the .htaccess file within that directory
   
   `vi /usr/share/phpMyAdmin/.htaccess`
   
   Within this file, we need to enter the following information:
   
   ```
   AuthType Basic
   AuthName "Provide your credentials"
   AuthUserFile /etc/httpd/pma_pass
   Require valid-user
   ```
    
    Now that we have specified the location for our password file through the use of the `AuthUserFile` directive in our `.htaccess` file, we need to create and populate the password file
    
    `htpasswd -c /etc/httpd/pma_pass username`
    
    The `-c` flag indicates that this will create an initial file. The directory location is the path and filename
     that will be used for the file. The `username` is the first user we would like to add. You will be prompted to enter and confirm a password for the user
     
     If you want to add additional users to authenticate, you can call the same command again **without** the -c flag, and with a new username
     
     `htpasswd /etc/httpd/pma_pass seconduser`
    
