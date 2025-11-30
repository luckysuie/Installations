## Install Prometheus & Grafana Natively (Binary Installation)
- Best for production environments

- Create a ubuntu VM with 2vcpus and 8 GB memory atleast with all ports open
- Login to VM using SSH and do the below

### Install Prometheus (Binary)-execute one after another below commands
```bash
sudo apt update
cd /opt/
sudo wget https://github.com/prometheus/prometheus/releases/download/v3.5.0/prometheus-3.5.0.linux-amd64.tar.gz
sudo tar -xvzf prometheus-3.5.0.linux-amd64.tar.gz
sudo mv prometheus-3.5.0.linux-amd64 prometheus
```
### Create a service file:
```bash
cd #make sure you do this cd because we need to get out of /opt folder
sudo nano /etc/systemd/system/prometheus.service
```
- Paste the below
```bash
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=root
ExecStart=/opt/prometheus/prometheus \
  --config.file=/opt/prometheus/prometheus.yml \
  --storage.tsdb.path=/opt/prometheus/data

[Install]
WantedBy=default.target
```
- save and exit (CTRL+O Then enter and then ctrl+x)

### Start service:
```bash
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
```
- Browse the public ip of VM with 9090 port Ex: http://VM-IP:9090



