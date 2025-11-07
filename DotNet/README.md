# .Net SDK installation and verification
------------------

sudo apt update
sudo apt-get update && sudo apt-get install -y wget apt-transport-https
wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get install -y dotnet-sdk-8.0
dotnet --version
