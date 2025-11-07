# Jenkins installation and verification
------------------

sudo apt update
sudo apt install openjdk-21-jdk -y # Jenkins needs Java to run
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \n  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \n  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \n  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl status jenkins
