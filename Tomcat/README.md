## Tomcat installation and verification
------------------
```bash
sudo apt update
sudo apt install openjdk-21-jdk -y
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.111/bin/apache-tomcat-9.0.111.tar.gz # change the URL if there is a new version
tar -xvzf apache-tomcat-9.0.111.tar.gz 
ls
mv apache-tomcat-9.0.111 tomcat
cd tomcat
cd bin
./startup.sh # to start tomcat server
./shutdown.sh # to stop tomcat server

vi ~/tomcat/webapps/manager/META-INF/context.xml
<Valve className=\"org.apache.catalina.valves.RemoteCIDRValve\"
        allow=\"127.0.0.0/8,::1/128\" /> # remove this section to allow access from all IPs
vi ~/tomcat/conf/tomcat-users.xml
    <role rolename=\"manager-gui\"/>
    <role rolename=\"manager-script\"/>
    <role rolename=\"manager-status\"/>
    <role rolename=\"admin-gui\"/>
    <user username=\"admin\" password=\"admin123\" roles=\"manager-gui,manager-script,manager-status,admin-gui\"/>
    # paste this user section before the closing </tomcat-users> tag
```
