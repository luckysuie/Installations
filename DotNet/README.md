## .Net SDK installation and verification
------------------
```bash
sudo apt update
sudo apt-get update && sudo apt-get install -y wget apt-transport-https
wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get install -y dotnet-sdk-8.0
dotnet --version
```

## DOTNET-7 & SDK and runtime Installation
```bash
sudo apt udpate
wget https://builds.dotnet.microsoft.com/dotnet/Sdk/7.0.410/dotnet-sdk-7.0.410-linux-x64.tar.gz 
  tar -xvzf dotnet-sdk-7.0.410-linux-x64.tar.gz
   ls -a
   vi .bashrc
```
- in the opened file at the bottom add this
```bash
export DOTNET_ROOT=$HOME
export PATH=$PATH:$HOME
```
save and Exit ESC then :wq
```bash
source .bashrc
dotnet --version
```
