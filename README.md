# OrangeHRM-in-CentOS-7
Installing OrangeHRM in CentOS 7

At first install updates 
$ yum update

Install Apache web server :
$ yum install httpd

Enable and start apache
$ systemctl enable httpd
$ systemctl start httpd
$ systemctl status status httpd

Install Mariadb and start it: 
$ yum install mariadb mariadb-server 
$ systemctl enable mariadb.service
$ systemctl status mariadb.service

Fix the mariadb setting so its safe to use it
$ mysql_secure_installation
You will be prompted for a root password . Use a strong password.
Next You will be asked a few questions . Just press Enter and it will be alright.

Lets create a OrangeHRM database: 
$ mysql -u root -p 
Enter the root password 
Now you will see the mysql command prompt. Type the following 
$ CREATE DATABASE orangehrm;
$ CREATE USER 'orangehrm_userz'@'localhost' IDENTIFIED BY 'PASSWORD';
$ GRANT ALL PRIVILEGES ON orangehrm.* TO 'orangehrm_userz'@'localhost';
$ FLUSH PRIVILEGES;
$ exit

Now we will install php
$ yum install php php-mysql
Go ahead and install anyother php module that you may need 

Now install a few things that we will need for OrangeHRM
$ yum install nano wget unzip

I will use nano you can use any other text editor like vim

$ mkdir Downloads
$ cd Downloads

Download OrangeHRM 
$ wget http://nchc.dl.sourceforge.net/project/orangehrm/stable/3.3.2/orangehrm-3.3.2.zip
$ unzip orangehrm-3.3.2.zip
$ ls 
There's a OrangeHRM folder 
Move it so we can use it
$ mv orangehrm-3.3.2/* /var/www/html && mv orangehrm-3.3.2/.htaccess /var/www/html 
 Change the owner 
 $ chown -R apache:apache /var/www/html
 Tweak a couple of setting so we can run it smoothly 
 $ nano /etc/my.cnf
 
 Under the [mysqld] block please place the following line: 
 # event_scheduler = ON 
 and then restart the MariaDB to apply the changes
$ systemctl restart mariadb

Save and close it.

$ nano /etc/httpd/conf/httpd.conf

Please find the line 
# ‘AllowOverride None‘ 
and change it to 
# ‘AllowOverride All‘
save the file and exit from text editor. This will enable .htaccess files to be used by your web browser. Now restart the apache web server to apply changes that we have just made.

$ systemctl restart httpd

Now Visit the IP address of your server and fill the form
# And voila OrangeHRM is installed !
