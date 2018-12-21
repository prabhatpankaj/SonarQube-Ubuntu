# SonarQube-Ubuntu

* Initial ubuntu setup

```
curl -sL https://raw.githubusercontent.com/prabhatpankaj/ubuntustarter/master/initial.sh | sh 

```
* Install Java

```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update -y
sudo apt-get install oracle-java8-installer -y


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
