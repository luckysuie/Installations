## Docker Installation and rootless access setup

- Install Docker (convenience script)
```bash
sudo apt update -y
wget -O docker.sh https://get.docker.com/
sudo sh docker.sh
```

- Ensure 'docker' group exists, then add your user
```bash
sudo groupadd -f docker
sudo usermod -aG docker "$USER"
```

- Restart Docker
```
sudo systemctl restart docker
newgrp docker
```

- Verify (no sudo)
```bash
docker --version
docker ps
```
