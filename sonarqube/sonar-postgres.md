
## SONARQUBE CONFIGURATION WITH POSTGRES DATABASE NEAR TO REAL TIME

- VM and Network Preparation
  - Create an Ubuntu 22.04 or 24.04 VM with at least 8 GB RAM and 2 vCPUs.
  - Open TCP port 9000 in your cloud firewall or security group.
________________________________________
Base Configuration as LakshmiNarayana@lucky
After logging into your VM do below
```bash
sudo apt update
sudo groupadd sonar
sudo groupadd postgres
sudo apt install openjdk-17-jdk -y
sudo useradd -m -s /bin/bash sonar -g sonar
sudo useradd -m -s /bin/bash postgres -g postgres
sudo passwd sonar
sudo passwd postgres
ls -a
sudo visudo
In the editor add these lines, then save and exit:
root    ALL=(ALL:ALL) ALL
sonar   ALL=(ALL:ALL) ALL
postgres ALL=(ALL:ALL) ALL
Continue:
su - sonar
sudo apt update
su - sonar
history
```
________________________________________
SonarQube Installation as sonar@lucky
In the sonar user you need to perform below activity
cd /opt
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-25.5.0.107428.zip
sudo apt install unzip
ls
unzip sonarqube-25.5.0.107428.zip
ls
mv sonarqube-25.5.0.107428 sonarqube
sudo nano /etc
ls -a
cd .
ls
cd /etc
ls
sudo vi sysctl.conf
Inside sysctl.conf add:
vm.max_map_count=262144
Then save and exit.
Continue:
cd /opt
ls
cd sonarqube/
ls
cd conf/
ls
sudo vi sonar.properties
Inside sonar.properties set:
sonar.search.javaOpts=-Xms512m -Xmx1g
Save and exit, then reboot to apply the kernel change.
sudo reboot
After reboot:
su - sonar
cd /opt
ls
cd sonarqube/
ls
cd bin/
ls
cd linux-x86-64/
ls
sh sonar.sh
sh sonar.sh start
ls
sh sonar.sh stop
history
At this point SonarQube runs but has no external database configured.
________________________________________
PostgreSQL Installation and Setup as postgres@lucky
After logging into postgres user you need to perform below means for logging into that
   su - postgres
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -
sudo apt install postgresql postgresql-contrib -y
sudo systemctl start postgresql
sudo systemctl enable postgresql
createuser sonar
psql
Inside the psql prompt:
ALTER USER sonar WITH ENCRYPTED password 'sonar';
CREATE DATABASE sonarqube OWNER sonar;
GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonar;
\q 	#exits the psql
Continue:
ls
ls -a
exit
history
________________________________________
Connect SonarQube to PostgreSQL as sonar@lucky
su - sonar
cd /opt/
ls
cd sonarqube/
ls
cd conf/
ls
sudo vi sonar.properties
Inside sonar.properties add or modify:
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube
sonar.search.javaOpts=-Xmx512m -Xms512m -XX:MaxDirectMemorySize=256m -XX:+HeapDumpOnOutOfMemoryError
Save and exit.
Continue:
cd
cd /opt/
ls
cd sonarqube/
cd bin/
ls
cd linux-x86-64/
ls
sh sonar.sh start
history
________________________________________
Verification
Open a browser to
http://<your-server-ip>:9000
Default login:
Username: admin
Password: admin
You’ll be prompted to change it on first login.
________________________________________
SonarQube 25.5 with PostgreSQL integration is now fully configured and running on your Ubuntu VM with port 9000 open and persistent database storage.
