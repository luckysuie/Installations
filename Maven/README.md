# Maven configuration and path setting
------------------

sudo apt update
# change the URL if there is a new version
wget https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz
tar -xvzf apache-maven-3.9.9-bin.tar.gz
ls
mv apache-maven-3.9.9 maven
cd maven
pwd
cd
ls -a
vi .bashrc

export MAVEN_HOME=/home/LakshmiNarayana/maven	# in place of LakshmiNarayana use your username
export PATH=$MAVEN_HOME/bin:$PATH

source ~/.bashrc
sudo apt install openjdk-21-jdk -y
mvn -v
