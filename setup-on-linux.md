# Identity-IIQ Installation on Ubuntu

## Pre-requisites:

### Install MySQL server 5.6 or above
```
sudo apt-get update
sudo apt-get install mysql-server
```

If the secure installation utility does not launch automatically after the installation completes, enter the following command:

```
sudo mysql_secure_installation utility
```

This utility prompts you to define the mysql root password and other security-related options, including removing remote access to the root user and setting the root password.

For more details on MySQL setup, refer the below link:
https://support.rackspace.com/how-to/install-mysql-server-on-the-ubuntu-operating-system/

### Setup Apache tomcat
```
sudo apt-get update
sudo apt-get install apache2

Configure the default UFW firewall to allow traffic on port 80.
sudo ufw allow 'Apache'
```

Verify apache installation:
```
http://localhost or http://127.0.0.1
``` 

For more details on Apache setup, refer the below link:
https://phoenixnap.com/kb/how-to-install-apache-web-server-on-ubuntu-18-04

### Setup Open-Jdk
```
sudo apt-get update
sudo apt-get install openjdk-8-jdk
```

Verify java version
```
java -version
```

If the correct version of Java is not being used, use the alternatives command to switch it:
```
sudo update-alternatives --config java

```

For more details on Apache setup, refer the below link:
https://docs.datastax.com/en/jdk-install/doc/jdk-install/installOpenJdkDeb.html
https://docs.oracle.com/javase/8/docs/technotes/guides/install/linux_jdk.html#BJFJJEFG

## Setting up Sailpoint
Download identityiq.zip (Version 8.1 is the latest one) from Sailpoint compass

```
sudo mkdir -p /opt/sailpoint/tomcat/webapps/iiq81
sudo cp identityiq.war /opt/sailpoint/tomcat/webapps/iiq81/
sudo su
cd /opt/sailpoint/tomcat/webapps/iiq81/
sudo jar xvf identityiq.war
```

### Build IIQ schema for mysql database
Execute the below commands from the bin directory:

```
cd /opt/sailpoint/tomcat/webapps/iiq81/WEB-INF/bin
chmod 777 iiq
./iiq schema
```

### Configure the repository for IdentityIQ 8.1(MySQL)
Go to the database directory, login to mysql database and load the schema into MySQL,
by executing the below commands:

```
cd /opt/sailpoint/tomcat/webapps/iiq81/WEB-INF/database
mysql -uroot -p<rootpassword>
source create_identityiq_tables-8.1.mysql
```

Once the schema is loaded, exit from the mysql and navigate to bin directory again and execute the below commands from the `iiq-console`:

```
cd /opt/sailpoint/tomcat/webapps/iiq81/WEB-INF/bin
./iiq console
import init.xml
import init-lcm.xml
```

### Restart the Tomcat server and launch iiq in the browser

```
sudo systemctl restart tomcat.service
```

Login to Identity IQ at `http://localhost:8080/identityiq/` with `spadmin/admin` as credentials
