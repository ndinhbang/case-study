# Some commands with `yum` on Centos

1.  Search a package

    `yum search php-`

2.  Install package(s) 

    `yum install zip unzip`
    
3. Remove packages and dependencies

    `yum remove --setopt=clean_requirements_on_remove=1 php`
    
    or 
    
    `yum autoremove php`
    
    or if you installed `yum-utils` 
    
    `package-cleanup -q --leaves | xargs -l1 yum -y remove`
    
    _This grabs all of the dependencies that can be removed without affecting anything else and then removes them_ 
    
    *** Note: Always add `-q` option to `package-cleanup` to prevent this command to remove `yum` .And that's not
     what you
     want.

4. To find out whether a specific package is installed

    `yum list installed | grep unzip`
    
5. List all available repositories

    `yum repolist`
    
6. View a full history of YUM transactions

    `yum history`
    
7. View details of transactions concerning a given package such as `httpd` web server

    `yum history info httpd `
    
8. Get a summary of the transactions concerning `httpd` package

    `yum history summary httpd`
    
9. It is also possible to use a transaction **ID**, the command below will display details of the transaction ID **15**.

    `yum history info 15`
    
10. 