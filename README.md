# System Monitoring Stack

## Description

A lightweight monitoring stack for Linux systems using `Netdata`, `Prometheus`, and `Grafana`.
This setup allows you to collect, store, and visualize system metrics, with a preconfigured Grafana dashboard to quickly detect anomalies such as unusually high numbers of open sockets or TCP connections (e.g., during Slowloris-like attacks).

## Prerequisites
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
   
4. Start and enable the Netdata service:
   
   ```bash
      sudo systemctl start netdata
      sudo systemctl enable netdata
   ```
   
Netdata will now be accessible at: 👉 `http://<IP_HOST>:19999`.

## Getting Started

1. Clone this repository:

   ```bash
      git clone https://github.com/<username>/system-monitoring-stack.git
      cd system-monitoring-stack/monitoring
   ```
   
2. Start the monitoring stack:

   ```bash
      docker compose up -d
   ```
   
3. Access the services:
   - Prometheus → `http://localhost:9090`.
   - Grafana → `http://localhost:3000`.
4. Configure Grafana:
   - Add Prometheus as a new data source: URL → `http://prometheus:9090`.
   - Import the dashboard: `grafana-dashboard.json`.

## Example Dashboard

Once configured, you will obtain a Grafana dashboard showing system metrics such as:

- TCP connections (active and orphaned).
- Number of open sockets.
- Network throughput.
- CPU and memory usage.
  
<img width="1190" height="724" alt="Screenshot 2025-08-23 alle 18 45 33" src="https://github.com/user-attachments/assets/facc0117-1b52-437d-93b3-5a5b7e2c0e2f" />

<img width="1186" height="724" alt="Screenshot 2025-08-23 alle 18 46 11" src="https://github.com/user-attachments/assets/24554e9a-2048-4738-a264-4fa9ec86307b" />


