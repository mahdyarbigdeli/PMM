#First You need to install Prometheus/Grafana

```
sudo nano /etc/.my.cnf
[client]
user=exporter_user
password=exporter_password
sudo chown mysqld_exporter:mysqld_exporter /etc/.my.cnf
sudo chmod 600 /etc/.my.cnf

```

#Second You need to have mysql/percona
CREATE USER 'exporter_user'@'localhost' IDENTIFIED BY 'exporter_password';
GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'exporter_user'@'localhost';
FLUSH PRIVILEGES;


wget https://github.com/prometheus/node_exporter/releases/download/v1.6.0/node_exporter-1.6.0.linux-amd64.tar.gz

tar -xvzf node_exporter-1.6.0.linux-amd64.tar.gz
cd node_exporter-1.6.0.linux-amd64

```
sudo nano /etc/systemd/system/node_exporter.service
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```
/usr/local/bin/mysqld_exporter --config.my-cnf=/etc/.my.cnf

sudo useradd -rs /bin/false node_exporter

sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl enable node_exporter
sudo systemctl status node_exporter


```
sudo nano /etc/prometheus/prometheus.yml
- job_name: $NAME-$IP
  metrics_path: /metrics
  scrape_interval: 5s
  static_configs:
  - targets:
    - exporter_IP:9104
```


