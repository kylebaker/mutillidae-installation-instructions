# Mutillidae Install Instructions

Instructions for both Windows and Linux to install Mutillidae from a fresh install. Most instrcutions are old or only available in video form. This is me filling that gap.

## Windows Install

To save us time, we are going to install everything we can from Chocolatey. 

First open Powershell as Administrator, and run:

`Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

Next, install xampp, notepad++ (to change config files later), and git:

`cinst -y bitnami-xampp notepadplusplus git`

Cause it's windows we can't just refresh the environment. Close and reopen Powershell (no need for `as admin` this time)

Download Mutillidae to the right folder:

`cd C:\xampp\htdocs`

And install mutillidae:

`git clone https://git.code.sf.net/p/mutillidae/git mutillidae`

Open the following file in NotePad++:

`C:\xampp\apache\conf\extra\httpd-xampp.conf`

And change `AllowOverride None` to `AllowOverride All`

Next, open the xampp control panel, start the mysql service, and hit the shell button in the GUI. 

Once there run:

`mysql -u root`

`use mysql;`

`SET PASSWORD FOR 'root'@'localhost' = PASSWORD('mutillidae');`

`flush privileges;`

`exit`

Once you do that MOST the errors are resolved. 

To remove the last one, edit this with NotePad++:

`C:\xampp\htdocs\mutillidae\owasp-esapi-php\lib\apache-log4php\trunk\src\main\php\helpers\LoggerPatternParser.php`

Change line 156 from `continue;` to `continue 2;`

Next, open a browser and go to:

`[ip of server]/mutillidae`

Click the blue link that says `setup/reset the DB`

With the pop up that comes next select `ok`

It is done

## Install Mutillidae on Linux (Ubuntu Server 18.04 LTS)

Make sure you are root for all of the next commands. Do so by running this:

`sudo su -`

Next upgrade the base OS and packages

`apt update && apt upgrade -y`

Install the necessary packages: 

`apt install -y apache2 php libapache2-mod-php mysql-server php-mysql php7.2-xml php7.2-curl php7.2-mbstring git`

Make sure to enable the apache2 module by running this:

`systemctl restart apache2`

`a2enmod rewrite`

=====

Change some file configurations. For these commands I'll use vim, but use whatever you are comfortable with:

`vim /etc/apache2/apache2.conf`

search for the Directory section where it's `/var/www/`

Change `AllowOverride` from `None` to `All` 

--

Change the password in MySQL, run these commands one at a time:

`mysql -u root`

`use mysql;`

`update user set authentication_string=PASSWORD('mutillidae') where user='root';`

`update user set plugin='mysql_native_password' where user='root';`

`flush privileges;`

`exit`

====

Install Mutillidae

`cd /var/www/html/`

`git clone https://git.code.sf.net/p/mutillidae/git mutillidae`

===

Now restart apache2:

`systemctl restart apache2`

and go to:

[ip of server]/mutillidae

Click the blue link that says `setup/reset the DB`

With the pop up that comes next select `ok`

It is done

===

## Install Mutillidae on Linux (Ubuntu Server 19.10 on Raspberry Pi 3)

The same as above, but change the packages to the following:

`apt install -y apache2 php libapache2-mod-php mysql-server php-mysql php7.3-xml php7.3-curl php7.3-mbstring git`

And for the MySQL part, use these commands instead of the ones listed above:

`ALTER USER 'root'@'localhost' IDENTIFIED BY 'mutillidae';`

`update user set plugin='mysql_native_password' where user='root';`

Everything else should be the same.
