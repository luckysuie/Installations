## Docker Installation and rootless access setup

```bash
sudo apt-get update
wget -qO- https://get.docker.com/ | sh ### Install Docker using the convenience script provided by Docker
sudo groupadd -f docker ## Create the docker group if it doesn't exist
sudo usermod -aG docker $USER ## Add the current user to the docker group
sudo systemctl restart docker
newgrp docker
docker --version  ## Verify that Docker is installed and running
```
