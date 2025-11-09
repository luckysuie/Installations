## Sonarqube as a Docker Container
- Create an Ubuntu 22.04 or 24.04 VM with at least 8 GB RAM and 2 vCPUs.
- Open TCP port 9000 in your cloud firewall or security group.

Login to VM and perform below

```bash
sudo apt update
sudo apt install docker.* -y
sudo docker pull sonarqube
sudo docker run -d -p 9000:9000 sonarqube
```
