# 🌐 LAMP Stack Setup on AWS EC2

## 📌 Introduction

A LAMP stack is a combination of open-source technologies used to build and deploy web applications. It includes:

* **Linux** – Operating System
* **Apache** – Web Server
* **MySQL** – Database Management System
* **PHP** – Server-side scripting language

This project demonstrates how I set up and configured a LAMP stack on an AWS EC2 instance.

---

## 🚀 Step 1: Launch EC2 Instance

* Created an EC2 instance named **Kay Web Server**
* Instance type: `t3.micro`
* OS: Ubuntu 24.04
* Region: `us-east-1`

### 🔐 Key Pair & Security Group

* SSH (Port 22) – for remote access
* HTTP (Port 80) – for web traffic
* HTTPS (Port 443) – for secure traffic

![Instance Launch](images/Instance%20Launch.png)

---

## 🔗 Step 2: Connect to EC2 Instance

```bash
chmod 400 kaylamp.pem
ssh -i kaylamp.pem ubuntu@your-public-ip
```

![Connect Instance](images/connect%20instance.png)

---

## 🌍 Step 3: Install Apache

Run this command to install and upadte list of packages in package manager:
```bash
sudo apt update
sudo apt install apache2
```
![install apache](images/install%20apache.png)
![install apache2](images/install%20apache2.png)

**Start and verify:**
The following commands were run to enable and verify that apache was running correctly. It indicated "green" and "running" showing it was correctly installed.
```bash
sudo systemctl start apache2
sudo systemctl status apache2
```

![startenable apache](images/startenable%20apache.png)

**Access the Server:**
The server can be accessed locally in the ubuntu shell or using the url on the browser.

```bash
curl http://localhost:80
curl http://127.0.0.1:80
```

![ubuntu page](images/ubuntu%20page.png)

This shows that the web server is correctly installed and accessable through the firewall.

---

## 🗄️ Step 4: Install MySQL

MySQL, a popular relational database management system which is to be used within PHP environment was installed by running this command:

```bash
sudo apt install mysql-server
```

![install mysql](images/install%20mysql.png)
![install mysql2](images/install%20mysql2.png)

When prompted, the installation was confirmed by typing y and enter.

**Enable and Verify MySQL Status:**

```bash
sudo systemctl enable mysql
sudo systemctl status mysql
```

![mysql status](images/mysql%20status.png)

It indicated "green" and "active(running)" showing it was correctly installed.

**Log in to MySQL:**
This is to connect to the server as the administrative root user using sudo command:

```bash
sudo mysql
```

**Set Password:**
When logged in to the mysql console, the password was set. The option of 2(STRONG) password installation was selected, and the prompts of Yes, No, Yes and Yes were selected.

```bash
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your_password';
```

![set password](images/set%20password.png)
![set password2](images/set%20password2.png)

**Log in With the New Password:**

After changing the root user password, log in with the reset password:

```bash
sudo mysql -u root -p
```

![log2 password](images/log2%20password.png)

**Exit MySQL Shell:**
Enter exit to log out

```bash
exit
```

![exit2 mysql](images/exit2%20mysql.png)

---

## 🐘 Step 5: Install PHP

**Install PHP and Necessary Modules:**
The following components were installed:
* *php package*
* *php-mysql:* a php module thst allows php to communicate with Mysql-based database.
* *libapache2-mod-php:* this enables apache to handle php files.

```bash
sudo apt install php libapache2-mod-php php-mysql
```

![install php](images/install%20php.png)
![install php2](images/install%20php2.png)

**Confirm installation:**

```bash
php -v
```

![confirm2 php](images/confirm2%20php.png)

At this point, the LAMP Web stack is successfully installed and fully operation.

This set up was tested with a PHP script. Apache Virtual Host was set up to hold website files. This allows multiples websites to be located on single machine and without being noticed by the website users.

## 📁 Step 6: Create Apache Virtual Host

**Create directory:**
/var/www/html is the default directory serving apache default page. I created my document(Kaylamp) directory next to the default one using 'mkdir' command:

```bash
sudo mkdir /var/www/kaylamp
sudo chown -R $USER:$USER /var/www/kaylamp
```

![creating directory](images/creating%20directory.png)

Create & Open A Sample Page:
Create and open a sample (info) page using a text editor(nano):

```bash
sudo nano /var/www/html/kaylamp
```

Paste this simple code there:

```bash
<?php
phpinfo();
?>
```

To access the document's info:

```bash
cat /var/www/html/kaylamp
```

![access phpinfo](images/access%20phpinfo.png)

**Create config file:**
Create and open a virtual host in apache configuration folder using nano.

```bash
sudo nano /etc/apache2/sites-available/kaylamp.conf
```

Add:

```apache
<VirtualHost *:80>
    ServerName kaylamp
    ServerAlias www.kaylamp
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/kaylamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

![project code](images/project%20code.png)

Enable site:

```bash
sudo a2ensite kaylamp.conf
sudo systemctl reload apache2
```

After reloading the Apache, go back to the web browser and revist our directory:

```bash
http://100.58.104.78/kaylamp/info.php
```

![newphp info](images/newphp%20info.png)

This is confirms that it is working perfectly.

---

## ✅ Result

I have successfully deployed a working LAMP stack on AWS EC2.

---

## 🎯 What I Learned

* AWS EC2 instance management
* Linux server configuration
* Web server (Apache) setup
* Database (MySQL) configuration
* PHP integration

---

## 📚 Read More!

* [Amazon EC2 Documentation](https://docs.aws.amazon.com/ec2/)
* [Apache Docs](https://httpd.apache.org/docs/)
* [MySQL Docs](https://dev.mysql.com/doc/)
* [PHP Docs](https://www.php.net/docs.php)
