# Java Web App Deployment on AWS EC2 + MySQL 

## 1. Lab Objective
- Launch 2 EC2 instances
- Configure MySQL database
- Deploy Java Web App on Tomcat
- Connect App VM → DB VM
- Verify registration & login

## 2. Architecture Overview
App Server: App-vm

DB Server: DB-vm

Flow: Browser → App VM (8080) → DB VM (3306)

## 3. Launch EC2 Instances
DB VM:

- Name: DB-VM

- Security Group:

SSH (22) → My IP

MySQL (3306) → sg-app-vm

![preview](./db10.png)

App VM:

- Name: App-vm

- Security Group:

SSH (22) → My IP

HTTP (8080) → Anywhere

![preview](./db9.png)

## 4. Configure DB VM


### Install MySQL:
    sudo apt update -y
    sudo apt install mysql-server -y
    sudo systemctl start mysql
    sudo systemctl enable mysql

![preview](./db12.png)

### Create Database:
    CREATE DATABASE jet;

    USE jet;

    Create Table:
    CREATE TABLE USER (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    username VARCHAR(50),
    password VARCHAR(50),
    regdate DATE
    );

![preview](./db13.png)

### Create User:
    CREATE USER 'appuser'@'%' IDENTIFIED BY 'YourPassword123!';

    GRANT ALL PRIVILEGES ON jet.* TO 'appuser'@'%';

    FLUSH PRIVILEGES;

![preview](./db1.png)

### Enable Remote Access:
    sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
### Change:

bind-address = 127.0.0.1

 To:

bind-address = 0.0.0.0

![preview](./db14.png)

### Restart MySQL:
    sudo systemctl restart mysql

Verify: sudo ss -tlnp | grep 3306

![prview](./db15.png)

## 5. Configure App VM
### Install Java & Maven:
    sudo apt install openjdk-11-jdk -y

    sudo apt install maven -y
![preview](./db16.png)

![preview](./db17.png)

### Install Tomcat:

cd /opt

    wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.91/bin/apache-tomcat-9.0.91.tar.gz

    tar -xvf apache-tomcat-9.0.91.tar.gz

    mv apache-tomcat-9.0.91 tomcat9

![preview](./db18.png)

    chmod +x /opt/tomcat9/bin/*.sh

### Fix Permissions:

    sudo chown -R ubuntu:ubuntu /opt/tomcat9

### Start Tomcat:

    sudo /opt/tomcat9/bin/startup.sh

![preview](./db19.png)

## Clone Project:
    git clone https://github.com/Akiranred/aws-rds-java.git

    cd aws-rds-java
 
 ![preview](./db20.png)

### Update DB Connection in JSP:

Class.forName("com.mysql.cj.jdbc.Driver");

Connection con = DriverManager.getConnection( "jdbc:mysql://:3306/jet?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC", "appuser", "YourPassword123!" );


### Build Project:


    mvn clean package

![preview](./db7.png)

### Deploy:
    sudo /opt/tomcat9/bin/shutdown.sh
    sudo rm -rf /opt/tomcat9/webapps/LoginWebApp*
    sudo cp target/LoginWebApp.war /opt/tomcat9/webapps/
    sudo /opt/tomcat9/bin/startup.sh

![preview](./db5.png)

### Test Application:
    http://<APP-IP>:8080/LoginWebApp

![preview](./db8.png)

### Verify Data:

mysql -u appuser -p -e "SELECT * FROM jet.USER;"

![preview](./db21.png)

### Summary:

- App deployed successfully

- DB connected

- Data stored and retrieved
