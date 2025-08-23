# System Monitoring Stack

## Description

A lightweight monitoring stack for Linux systems using `Netdata`, `Prometheus`, and `Grafana`.
This setup allows you to collect, store, and visualize system metrics, with a preconfigured Grafana dashboard to quickly detect anomalies such as unusually high numbers of open sockets or TCP connections (e.g., during Slowloris-like attacks).

## 🔧 Prerequisites
Before starting, you need to install and configure Netdata on the host machine.

1. Install Netdata:
   ```bash
      sudo apt install netdata
   ```
2. Generate the default configuration (if not already present):
   ```bash
      sudo /usr/sbin/netdata -W build-config
   ```
3. Edit the Netdata configuration file:
   ```bash
      sudo vi /etc/netdata/netdata.conf
   ```
   In the section `[global]` or `[web]`, set:
   ```bash
      bind socket to IP = 0.0.0.0
   ```
   (or, more restrictively, `bind socket to IP = <IP_HOST>`).
5. Start and enable the Netdata service:
   ```bash
      sudo systemctl start netdata
      sudo systemctl enable netdata
   ```
Netdata will now be accessible at: 👉 `http://<IP_HOST>:19999`.
