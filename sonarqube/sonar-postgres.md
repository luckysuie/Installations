
## SONARQUBE CONFIGURATION WITH POSTGRES DATABASE NEAR TO REAL TIME

### Little learning 
1. What is /opt and /etc in Linux?
<p>/opt — “Optional software”
It stands for optional and is used to install third-party or custom software.
Unlike /usr/bin (which is for system packages installed via apt), /opt is usually used for manual installations — like SonarQube, Jenkins, or IntelliJ. </p>

<p>/etc — “Editable Text Configuration”
/etc holds configuration files for almost all software and services.
- Example analogy:
/etc = your system’s “Settings” folder — where you change how things run.</p>


2. What is Elasticsearch (and why is it important in SonarQube)?
- (ES) is a search and analytics engine.
- It indexes and queries large volumes of data very fast.
- How SonarQube uses it:
  - When SonarQube scans code, it stores results (issues, metrics, code smells, bugs) in a database.
  <p>But for searching, filtering, and analytics, it uses Elasticsearch because it’s optimized for:</p>
    
    - Full-text search
    - Fast filtering (e.g., “show all issues of severity = Critical in project X”)
    - Aggregations (metrics, charts)

3. Why PostgreSQL Database is needed (and what it does)
<p>Initially, SonarQube comes with an embedded H2 database — which is temporary and not reliable for real deployments.
You replaced it with PostgreSQL, which is a production-grade relational database.</p>

- What it stores:
  - Projects, issues, metrics, scan history
  - User accounts, roles, permissions
  - Plugin configurations and analysis results
  - Background tasks and processing queues

## VM and Network Preparation
  - Create an Ubuntu 22.04 or 24.04 VM with at least 8 GB RAM and 2 vCPUs.
  - Open TCP port 9000 in your cloud firewall or security group.
________________________________________
- Base Configuration as LakshmiNarayana@lucky
- After logging into your VM do below. Execute one after another
```bash
sudo apt update
sudo groupadd sonar
sudo groupadd postgres
sudo apt install openjdk-17-jdk -y
sudo useradd -m -s /bin/bash sonar -g sonar
sudo useradd -m -s /bin/bash postgres -g postgres
sudo passwd sonar
sudo passwd postgres
sudo apt install unzip
sudo chmod 777 /opt
```
```bash
sudo visudo
```
- In the editor add these lines, then save and exit:
```bash
root    ALL=(ALL:ALL) ALL
sonar   ALL=(ALL:ALL) ALL
postgres ALL=(ALL:ALL) ALL
```
________________________________________
- SonarQube Installation as sonar@lucky
- In the sonar user you need to perform below activity. Execute one after another
```bash
su - sonar
cd /opt
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-25.5.0.107428.zip
ls
unzip sonarqube-25.5.0.107428.zip
ls
mv sonarqube-25.5.0.107428 sonarqube
cd
cd /etc
ls
sudo vi sysctl.conf
```
Inside sysctl.conf add:
```bash
vm.max_map_count=262144
```
- Then save and exit.
  - Continue:
```bash
cd  
cd /opt
ls
cd sonarqube/
ls
cd conf/
ls
sudo vi sonar.properties
```
- Inside sonar.properties set:
```bash
sonar.search.javaOpts=-Xms512m -Xmx1g
```
- Save and exit, then reboot to apply the kernel change.
```bash
sudo reboot
```
- After reboot:
```bash
su - sonar
cd /opt
ls
cd sonarqube/
ls
cd bin/
ls
cd linux-x86-64/
ls
sh sonar.sh start
```
- Browse http://yourpublicip:9000
```bash 
ls
sh sonar.sh stop # it will stopsonarqube
```
- At this point SonarQube runs but has no external database configured.
________________________________________
- PostgreSQL Installation and Setup as postgres@lucky
- After logging into postgres user you need to perform below means for logging into that
```bash
su - postgres
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -
sudo apt install postgresql postgresql-contrib -y
sudo systemctl start postgresql
sudo systemctl enable postgresql
createuser sonar
psql
```
- Inside the psql prompt(Execute one after another):
```bash
ALTER USER sonar WITH ENCRYPTED password 'sonar';
CREATE DATABASE sonarqube OWNER sonar;
GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonar;
\q 	#exits the psql
```
- Continue:
```bash
exit
```

- Connect SonarQube to PostgreSQL as sonar@lucky
```bash
su - sonar
cd /opt/
ls
cd sonarqube/
ls
cd conf/
ls
sudo vi sonar.properties
```
Inside sonar.properties add or modify:
```bash
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube
sonar.search.javaOpts=-Xmx512m -Xms512m -XX:MaxDirectMemorySize=256m -XX:+HeapDumpOnOutOfMemoryError
```
Save and exit.
Continue:
```bash
cd
cd /opt/
ls
cd sonarqube/
cd bin/
ls
cd linux-x86-64/
ls
sh sonar.sh start
```
- Verification
  - Browse http://yourpublicip:9000
  - Default login:
  - Username: admin
  - Password: admin
- You’ll be prompted to change it on first login.
________________________________________
- SonarQube 25.5 with PostgreSQL integration is now fully configured and running on your Ubuntu VM with port 9000 open and persistent database storage.


<img width="1894" height="868" alt="image" src="https://github.com/user-attachments/assets/438359f2-decc-448c-8dfc-f7c67aa508df" />

