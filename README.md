# SonarQube-Ubuntu

* Initial ubuntu setup

```
curl -sL https://raw.githubusercontent.com/prabhatpankaj/ubuntustarter/master/initial.sh | sh 

```
* Install Java

```
sudo add-apt-repository ppa:webupd8team/java

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

```
