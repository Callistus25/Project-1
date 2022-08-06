## WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
### What is a Technology stack?
A technology stack is a set of frameworks and tools used to develop a software product. This set of frameworks and tools are very specifically chosen to work together in creating a well-functioning software. They are acronymns for individual technologies used together for a specific technology product. some examples are…

LAMP (Linux, Apache, MySQL, PHP or Python, or Perl)
LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl)
MERN (MongoDB, ExpressJS, ReactJS, NodeJS)
MEAN (MongoDB, ExpressJS, AngularJS, NodeJS.
 ### Guide
 Setup your instance and follow the below steps
 
1. Step 1: INSTALLING APACHE AND UPDATING THE FIREWALL **
  - update a list of packages in package manager and run apache2 package installation
  ```
  sudo apt update
  sudo apt install apache2
  ```
  ![sudo apt update](https://user-images.githubusercontent.com/107906178/183227445-6ede334f-49fd-409b-8459-52e225d50c5a.jpg)
  
  - Verify that apache2 is running as a Service in our OS.
  ```
  sudo systemctl status apache2
  ```
  ![sudo systemctl status apache2](https://user-images.githubusercontent.com/107906178/183227482-05f21e40-7688-46fa-bf49-785778d6e986.jpg)
  
  Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is the default port that web browsers use to access web pages on the Internet by adding a rule to EC2 configuration to open inbound connection through port 80.
  ![EC2 Port 80 addition](https://user-images.githubusercontent.com/107906178/183227882-cb110184-7aed-49bb-bd48-8df6e3f545a2.JPG)
  
  - To confirm that the server is running and can be accessed locally and from the Internet (Source 0.0.0.0/0 means ‘from any IP address’). run the below command
  ```
  curl http://localhost:80
or
 curl http://127.0.0.1:80
 ```
 ![curl http Localhost 80](https://user-images.githubusercontent.com/107906178/183227949-bb2f6539-10a5-452f-8bba-e87d6dcf106f.jpg)
 
 To test how our Apache HTTP server can respond to requests from the Internet, open a web browser run the IP address through port 80.
 ```
 http://<Public-IP-Address>:80
 
 ```
  Another way to retrieve your Public IP address, other than to check it in AWS Web console, is to use following command:
 ```
 curl -s http://169.254.169.254/latest/meta-data/public-ipv4
 ```
 ![Public IP test](https://user-images.githubusercontent.com/107906178/183228008-6eabb948-6508-4c14-95e7-8a3d13c3e9a6.jpg)
 
 2. STEP 2 : INSTALLING MYSQL
 - With a web server up and running, then install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database.
```
 sudo apt install mysql-server
 ```
 ![Sudo apt install mysql-server](https://user-images.githubusercontent.com/107906178/183228135-ef119f5d-853f-4f04-b07a-1853b4d4b17d.JPG)
 
 - When the installation is finished, log in to the MySQL console
 ```
 sudo mysql
 ```
 ![sudo my sql](https://user-images.githubusercontent.com/107906178/183228141-46b57b7f-92e3-4fd8-862d-2e5de95dc5ab.JPG)
 
 It is important to run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system. Before running the script you will set a password for the root user, using mysql_native_password as default authentication method. 'Password.1' was used as password in this case. For the rest of the questions, press Y and hit the ENTER key at each prompt. This will prompt you to change the root password, remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately respects the changes you have made.

- Exit the MySQL shell 
 ```
 mysql > exit
 ```
 - Start the interactive script by running:
 ```
 sudo mysql_secure_installation
 ```
 This will ask if you want to configure the VALIDATE PASSWORD PLUGIN. Answer Y for yes, or anything else to continue without enabling. If you answer “yes”, you’ll be asked to select a level of password validation.
- When you’re finished, test if you’re able to log in to the MySQL console by typing the below command which will prompt you for password.
```
sudo mysql -p
```
![sudo mysql -p](https://user-images.githubusercontent.com/107906178/183228146-39b8afc7-ad1d-442f-839e-05908fc441b3.JPG)

- Exit the MySQL console. MySQL server is now installed and secured.

3. STEP 3 : INSTALLING PHP
PHP is the component of our setup that will process code to display dynamic content to the end user. In addition to the php package, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. You’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.
To install these 3 packages at once, run the below command
```
sudo apt install php libapache2-mod-php php-mysql
```
Once the installation is finished, you can run the following command to confirm the PHP version:
```
php -v
```
![php - v](https://user-images.githubusercontent.com/107906178/183228149-a248ed2c-2af6-402e-a542-07212dd32709.JPG)

 
At this point, your LAMP stack is completely installed and fully operational.

Linux (Ubuntu)
Apache HTTP Server
MySQL
PHP
To test your setup with a PHP script, it’s best to set up a proper Apache Virtual Host to hold your website’s files and folders. Virtual host allows you to have multiple websites located on a single machine and users of the websites will not even notice it.

![VirtualHost](https://user-images.githubusercontent.com/107906178/183229409-3c213e8f-f1f4-4101-83ee-900eb1e1efc9.png)

4. STEP 4 : CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE
- Create the directory for projectlamp using ‘mkdir’ command and assign ownership of the directory with your current system user. Afterwards, create and open a new configuration file in Apache’s sites-available directory using vim line editor.
```
sudo mkdir /var/www/projectlamp
sudo chown -R $USER:$USER /var/www/projectlamp
sudo vi /etc/apache2/sites-available/projectlamp.conf
```
![sudo vi ](https://user-images.githubusercontent.com/107906178/183228151-35916d08-9d46-43a3-9877-690bd8f97875.jpg)

This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text, save and close the file.
```
<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
You can use the ls command to show the new file in the sites-available directory
```
sudo ls /etc/apache2/sites-available
```
![sudo vi ](https://user-images.githubusercontent.com/107906178/183228151-35916d08-9d46-43a3-9877-690bd8f97875.jpg)

With this VirtualHost configuration, Apache to serves projectlamp using /var/www/projectlampl as its web root directory.
- To enable the new virtual host, run the below command
```
sudo a2ensite projectlamp
```
- Disable the default website that comes installed with Apache. This is required if you’re not using a custom domain name, because in this case Apache’s default configuration would overwrite your virtual host. To disable Apache’s default website use a2dissite command , type:
``` 
sudo a2dissite 000-default 
```
- Make sure your configuration file doesn’t contain syntax errors, run:
```
sudo apache2ctl configtest
```
Finally, reload Apache so these changes take effect:
```
sudo systemctl reload apache2
```
The new website is now active, but the web root /var/www/projectlamp is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected:
```
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
```
- Now go to the browser and try to open your website URL using IP address
 ```
  http://<Public-IP-Address>
 ```
 ![Apache Virtual host lauching with Public IP address](https://user-images.githubusercontent.com/107906178/183228154-72d0bc06-9b7b-4224-91a0-a34facd59b50.jpg)
You can also access your website in your browser by public DNS name, not only by IP.
  ```
  http://<Public-DNS-Name>
  ```
  ![Apache Virtual host lauching with DNS name](https://user-images.githubusercontent.com/107906178/183228155-70045424-5443-4942-b0eb-7649b74a99f7.jpg)
  
- Leave this file in place as a temporary landing page for your application until you set up an index.php file to replace it. Once you do that, remember to remove or rename the index.html file from your document root, as it would take precedence over an index.php file by default.
  
  5. STEP 5 : ENABLE PHP ON THE WEBSITE
With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file. This is useful for setting up maintenance pages in PHP applications, by creating a temporary index.html file containing an informative message to visitors. Because this page will take precedence over the index.php page, it will then become the landing page for the application. Once maintenance is over, the index.html is renamed or removed from the document root, bringing back the regular application page.
```
  sudo vim /etc/apache2/mods-enabled/dir.conf
```
```
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```
After saving and closing the file, you will need to reload Apache so the changes take effect  
In case you want to change this behavior, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive:
  ```
  sudo systemctl reload apache2
  ```
 - Create a PHP script to test that PHP is correctly installed and configured on your server.
  Create a new file named index.php inside your custom web root folder:
  ```
  vim /var/www/projectlamp/index.php
  ```
  This will open a blank file. Add the following text, which is valid PHP code, inside the file:
  ```
  <?php
phpinfo();
```
When you are finished, save and close the file, refresh the page.
![PHP installation complete](https://user-images.githubusercontent.com/107906178/183228156-5f3ef9ed-1c03-4672-b818-50649ad4abf6.JPG)

After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment -and your Ubuntu server. You can use rm to do so:
```
sudo rm /var/www/projectlamp/index.php
```
