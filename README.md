# System Monitoring Stack
![Category](https://img.shields.io/badge/Category-Monitoring-blue)  
![Compatibility](https://img.shields.io/badge/Compatibility-Linux-orange)  
![Technologies](https://img.shields.io/badge/Technologies-Docker%20%7C%20Prometheus%20%7C%20Grafana%20%7C%20Netdata-brightgreen)  
![Features](https://img.shields.io/badge/Features-Real--time%20Metrics%20%7C%20Time--Series%20Storage%20%7C%20Custom%20Dashboards-green)  
![Prerequisites](https://img.shields.io/badge/Requires-Netdata-yellow)  

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
      git clone https://github.com/simona2606/system-monitoring-stack.git
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

## How it works

This stack leverages the modular architecture of Netdata, Prometheus, and Grafana to provide a robust and extensible monitoring solution. Each component plays a specific role in the ecosystem and, combined together, they allow real-time data collection, historical storage, and visualization of system metrics.

### Core Components 
- **Netdata:** Lightweight monitoring agent that runs on the host system. It collects real-time metrics about CPU, memory, disk I/O, network, and system processes, and exposes them via an HTTP endpoint.
- **Prometheus:** Time-series database that scrapes metrics from Netdata at regular intervals, stores them efficiently, and provides a powerful query language (PromQL) for analysis.
- **Grafana:** Visualization and analytics platform that connects to Prometheus as a data source. It enables the creation of interactive dashboards to explore metrics, detect anomalies, and correlate events.

### Networking & Orchestration
- **Docker Compose:** Orchestrates the Prometheus and Grafana containers, exposing their services on ports 9090 and 3000 respectively.
- **Netdata Endpoint:** Runs directly on the host and is made available on port 19999. Prometheus connects to this endpoint (/api/v1/allmetrics) to ingest data.

## FAQ

### Why do I need to install Netdata separately?  
Because Netdata runs as an agent directly on the host system. Prometheus then scrapes its metrics endpoint. Installing Netdata separately ensures you get full access to system-level data (CPU, memory, sockets, processes) that would not be available from inside a container.

### Can I use this stack on a remote server?  
Yes. You can deploy Prometheus and Grafana on a VPS or cloud server. Just make sure that Netdata is installed on the target host, that its endpoint is reachable, and that firewall/DNS settings are properly configured.

### Can I customize the dashboards?
Absolutely. The provided `grafana-dashboard.json` is a starting point. You can import it and then add, edit, or duplicate panels to suit your specific monitoring needs.
