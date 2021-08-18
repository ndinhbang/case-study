# How to install LAMP stack on CentOS 7

## Install Apache2

1. Install Apache2

    Apache is available in the default CentOS 7 repositories and the installation is pretty straight forward. To install the package run the following command

    ```bash
    yum install httpd
    ```

    Once the installation is completed, start and enable the Apache service by typing

    ```bash
   systemctl start httpd
   systemctl enable httpd
   ```

## Install MySQL

<https://tecadmin.net/install-mysql-8-on-centos/>

1. Enable the MySQL 5.7/8.0 repository with the following command

   ```bash
   yum localinstall https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
   ```

   or

   ```bash
   rpm -Uvh https://repo.mysql.com/mysql80-community-release-el7-3.noarch.rpm
   ```

2. Install MySQL package with yum

    ```bash
    yum install mysql-community-server
    ```

    or

    ```bash
    yum --enablerepo=mysql80-community install mysql-community-server
    ```

3. Once the installation is completed, start the MySQL service and enable it to automatically start on boot with

    ```bash
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

    ```bash
   Securing the MySQL server deployment.
   Enter password for user root:
   ```

   After entering the temporary password you will be asked to set a new password for user root. The password needs to
    be **at least 8-characters long and to contain at least one uppercase letter, one lowercase letter, one number, and one special character**.

    ```bash
   The existing password for the user account root has expired. Please set a new password.
   New password:
   Re-enter new password:
   ```

   Then the script will also ask you to remove the anonymous user, restrict root user access to the local machine and
    remove the test database. You should **answer “Y” (yes) to all questions**.

## Install PHP7

1. Install EPEL repo

    ```bash
    yum install epel-release
    ```

2. Install Remi repo

    ```bash
    yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
    ```

3. Install **yum-utils** - _a collection of useful programs for managing yum repositories and packages. It can be used
 for managing (enabling or disabling) yum repositories as well as packages without any manual configuration and so much more_.

    ```bash
    yum install yum-utils
    ```

4. Disable previous php repo

    ```bash
    yum-config-manager --disable 'remi-php*'
    ```

5. Enable Remi repository as the default repository for installing different PHP versions

    ```bash
    yum-config-manager --enable remi-php71
    yum-config-manager --enable remi-php72
    yum-config-manager --enable remi-php73
    yum-config-manager --enable remi-php74
    yum-config-manager --enable remi-php80
    ```

6. Now we can install several most common PHP modules

    ```bash
    yum install php php-common php-devel php-opcache php-cli php-mcrypt php-mbstring php-bcmath php-json php-mysqlnd
     php-event php-zip php-xml php-xmlrpc
    ```

### Install `libsodium`

 1. Install both the library and PHP extension
  
   ```bash
    yum install libsodium-devel
    yum install php-sodium
   ```

  Verify that you have `libsodium` installed

   ```bash
    php -m
   ```

## Install `redis`

<https://www.howtoforge.com/how-to-install-and-secure-redis-on-centos-7/>

```bash
yum --enablerepo=remi install redis
```

Start Redis Service on CentOS 7

```bash
systemctl start redis
systemctl enable redis
```

check the service status and the port used by the server

```bash
systemctl status redis
netstat -plntu
```

Configure Redis

```bash
vi /etc/redis.conf
```

change the 'daemonize' value to 'yes', because we will run the Redis service as a daemon.

```bash
daemonize yes
```

Since we're using the CentOS 7 server and systemd, so we need to change the 'supervised' line configuration to 'systemd'.

```bash
supervised systemd
```

Save and close `redis.conf`

Now restart the redis service.

```bash
systemctl restart redis
```

### Install `phpredis` extension

<https://github.com/phpredis/phpredis/blob/develop/INSTALL.markdown>

```bash
yum install php-pecl-redis
```

Restart your Apache server to ensure that it's working with the newly installed PHP:

```bash
systemctl restart httpd.service
```

Let's check php configuration. we need to create info.php file and put it on the default PHP directory `/var/www/html`

```bash
vi /var/www/html/info.php
```

Then, insert this code inside:

```php
<?php phpinfo(); ?>
```

The last thing is to check your server by visiting the info.php URL

```bash
http://your.ip.address/info.php
```

## Install `composer`

Switch into the temp directory.

```php
cd /tmp
```

Install Composer using cURL

```bash
curl -sS https://getcomposer.org/installer | php
```

Want to make Composer globally accessible?

```bash
mv composer.phar /usr/local/bin/composer
```

## Install `git`

<https://www.digitalocean.com/community/tutorials/how-to-install-git-on-centos-7>

```bash
yum install git
```

check Git built-in version

```bash
git --version
```

## Install `supervisor`

<https://cloudwafer.com/blog/how-to-install-and-configure-supervisor-on-centos-7/>

```bash
yum install supervisor
```

Then start and enable the supervisord daemon to start on boot using the commands below:

```bash
systemctl start supervisord
systemctl enable supervisord
```

check the status of the service with the command below

```bash
systemctl status supervisord
```

## Install `nodejs` & `npm`

### Install `nvm`

<https://github.com/nvm-sh/nvm#installing-and-updating>
<https://tecadmin.net/how-to-install-nvm-on-centos-7/>
NVM (Node Version Manager) is a bash script used to manage multiple active Node.js versions. NVM allows us to install and uninstall any specific Node.js version which means we can have any number of Node.js versions we want to use or test

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```

Basically, NVM keeps all files under the `$HOME/.nvm` directory. Then it sets environment in users `.bashrc` file. You need to load this environment to set required configuration by running the following command:

```bash
source ~/.bashrc
```

verify that nvm was properly installed

```bash
nvm --version
```

### Install node.js

install the latest LTS version

```bash
nvm install --lts
```

list all installed Node.js instances

```bash
nvm ls
```

To change the currently active version

```bash
nvm use <version>
```

check the node version installed:

```bash
node -v
```

### Install `yarn`

enable the Yarn repository and import the repository's GPG key

```bash
curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
rpm --import https://dl.yarnpkg.com/rpm/pubkey.gpg
```

```bash
yum install yarn
```

Verify the installation by printing the Yarn version number:

```bash
yarn --version
```
