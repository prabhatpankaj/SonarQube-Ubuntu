# SonarQube-Ubuntu

* Initial ubuntu setup

```
curl -sL https://raw.githubusercontent.com/prabhatpankaj/ubuntustarter/master/initial.sh | sh 

```

* Install Java

```
sudo apt update
sudo apt install openjdk-8-jdk
```
*  Setting the JAVA_HOME Environment Variable

Many programs written using Java use the JAVA_HOME environment variable to determine the Java
installation location.
To set this environment variable, first determine where Java is installed. Use the update-alternatives
command:
```
sudo update-alternatives --config java
```
* Copy the path from your preferred installation. Then open /etc/environment using nano or your
favorite text editor:

```
sudo nano /etc/environment
```
* At the end of this file, add the following line, making sure to replace the highlighted path with your
own copied path:

```
JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/"
```

* Modifying this file will set the JAVA_HOME path for all users on your system.

```

source /etc/environment
```

* Install Mysql

```
sudo apt-get -y install mysql-server mysql-client
sudo systemctl restart mysql
sudo systemctl enable mysql

```
* Install Apache2

```
sudo apt-get install apache2 -y
sudo systemctl restart apache2
sudo systemctl enable apache2

```

* Configure SonarQube user and database 

```
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '01@Pass1234';
\q

mysql -u root -p

CREATE DATABASE sonar;
CREATE USER sonar@'localhost' IDENTIFIED BY '01@Pass1234';
GRANT ALL ON sonar.* to sonar@'localhost';
FLUSH PRIVILEGES;
EXIT;

```
* Install SonarQube

```
sudo apt install zip -y
sudo adduser sonar
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-6.7.6.zip
unzip sonarqube-6.7.6.zip
sudo cp -r sonarqube-6.7.6 /opt/sonarqube
sudo chown -R sonar:sonar /opt/sonarqube
sudo nano /opt/sonarqube/bin/linux-x86-64/sonar.sh
```
* Configure SonarQube shell

```
sudo nano /opt/sonarqube/bin/linux-x86-64/sonar.sh

```
* Make the following changes:

```
RUN_AS_USER=sonar

```
Save and close the file, when you are finished.

* SonarQube default configuration file

```
sudo nano /opt/sonarqube/conf/sonar.properties
```
* Make the following changes:

```
sonar.jdbc.username=sonar
sonar.jdbc.password=01@Pass1234
sonar.jdbc.url=jdbc:mysql://localhost:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance&useSSL=false
sonar.web.host=0.0.0.0
sonar.search.javaOpts=-Xms512m \
 -Xmx512m \
 -XX:+HeapDumpOnOutOfMemoryError
```
Save and close the file, when you are finished.

* Create Systemd Service file for SonarQube

```
sudo nano /etc/systemd/system/sonar.service

```

* Add the following lines:

```
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking

ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always

[Install]
WantedBy=multi-user.target
```
Save and close the file, when you are finished.

* start SonarQube service and enable it to start on boot time

```
sudo systemctl start sonar
sudo systemctl enable sonar
```
* You can check the status of SonarQube service with the following command:

```
sudo systemctl status sonar
```
* visit SonarQube page

```
http://YOUR_PUBLIC_IP:9000
```

