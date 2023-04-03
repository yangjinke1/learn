# Monitor

## How to install prometheus?

Prometheus is an open-source monitoring and alerting tool that is widely used to monitor and track the performance of servers, applications, and services. Here are the steps to install Prometheus on Ubuntu 22:

```shell
wget https://github.com/prometheus/prometheus/releases/download/v2.30.3/prometheus-2.30.3.linux-amd64.tar.gz
tar xvfz prometheus-*.tar.gz
sudo mv prometheus-* /opt/prometheus

sudo useradd --no-create-home --shell /bin/false prometheus
sudo chown -R prometheus:prometheus /opt/prometheus
```

Create a systemd service file for Prometheus:

```ini
# sudo vim /etc/systemd/system/prometheus.service

[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/opt/prometheus/prometheus --config.file=/opt/prometheus/prometheus.yml --storage.tsdb.path=/opt/prometheus/data

[Install]
WantedBy=multi-user.target
```

Reload the systemd daemon and start the Prometheus service:

```shell
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl status prometheus
```

Open your web browser and navigate to <http://localhost:9090>. This will open the Prometheus web interface, where you can configure and manage your Prometheus instance.

## How to install Node exporter

Node exporter is a Prometheus exporter that collects system metrics from the Linux operating system. Here are the steps to install node-exporter on Ubuntu 22:

```shell
wget https://github.com/prometheus/node_exporter/releases/download/v1.2.2/node_exporter-1.2.2.linux-amd64.tar.gz
tar xvfz node_exporter-*.tar.gz
sudo mv node_exporter-* /opt/node_exporter

sudo useradd --no-create-home --shell /bin/false node_exporter

sudo chown -R node_exporter:node_exporter /opt/node_exporter
```

Create a systemd service file for node-exporter:

```ini
# sudo vim /etc/systemd/system/node_exporter.service
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/opt/node_exporter/node_exporter

[Install]
WantedBy=multi-user.target
```

Reload the systemd daemon and start the node-exporter service:

```shell
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl status node_exporter
```

Open your web browser and navigate to <http://localhost:9100/metrics>. This will display the system metrics collected by node-exporter in the Prometheus format.

## How to install Grafana

You can follow the steps below to install Grafana using the binary package "grafana-9.4.7.linux-amd64.tar.gz" on Ubuntu 22:

```shell
wget https://dl.grafana.com/oss/release/grafana-9.4.7.linux-amd64.tar.gz
tar -zxvf grafana-9.4.7.linux-amd64.tar.gz
sudo mv grafana-9.4.7 /opt/grafana
sudo useradd grafana
sudo chown -R grafana:grafana /opt/grafana
```

To add Grafana as a system service

```shell
# sudo vim /etc/systemd/system/grafana.service

[Unit]
Description=Grafana service
After=network.target

[Service]
User=grafana
Group=grafana
WorkingDirectory=/opt/grafana
ExecStart=/opt/grafana/bin/grafana-server --config=/etc/grafana/grafana.ini --pidfile=/var/run/grafana/grafana.pid --packaging=deb cfg:default.paths.logs=/var/log/grafana cfg:default.paths.data=/var/lib/grafana cfg:default.paths.plugins=/var/lib/grafana/plugins
Restart=always

[Install]
WantedBy=multi-user.target
```

Reload the systemd configuration files and start the Grafana service:

```shell
sudo systemctl daemon-reload
sudo systemctl enable grafana.service
sudo systemctl start grafana.service
sudo systemctl status grafana.service
```

Open your web browser and navigate to <http://localhost:3000>. This will open the Grafana login page.

Log in to Grafana using the default username and password, which are both "admin".

Follow the prompts to change your password and configure your Grafana instance.
