# SonarQube as a Docker container
------------------

sudo apt update
sudo apt install docker.io -y
sudo docker pull sonarqube
sudo docker run -d -p 9000:9000 sonarqube
