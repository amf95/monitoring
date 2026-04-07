

> **Note**: You can use containers or virtual machines for these services.

> **Note**: This tutorial teaches you how to install monitoring services on an offline machine manually and all the instructions can be turned into a script or **Ansible** playbook.

> **Note**: In this tutorial I'll be using virtual machines for the sake of simplicity.

> **Note**: Almost every major service has its own Prometheus exporter like: mySQL, Nginx, Haproxy, Apache, ...

---
## How everything works together?

![](1-prometheus/assets/prometheus-system-architecture-and-how-it-works.svg)

---
## Table of content:

#### Metrics Monitoring:

>Note: Prometheus **PULLS** data from the exporters every **X** amount of time.

| Job                       | Tool                         | Description                                                                                                  | Installation Location   |
| ------------------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------ | ----------------------- |
| Metrics Storage           | Prometheus                   | collect, store, label and expose metrics data form exporters.                                                | Monitoring Machine      |
| Endpoints Monitoring      | Prometheus Blackbox Exporter | monitor endpoints http/s, dns, ssl, ... and expose it to Prometheus.                                         | Monitoring Machine      |
| Machine Health Monitoring | Prometheus Node Exporter     | collect machine(node) info about cpu, memory, disk usage, systemd services, ... and expose it to Prometheus. | Monitored Machine(Node) |

---
#### GUI-Display:


| Job                    | Tool    | Description                                                     | Installation Location |
| ---------------------- | ------- | --------------------------------------------------------------- | --------------------- |
| Display Collected Data | Grafana | integrates with **Prometheus** to display their collected data. | Monitoring Machine    |

---
#### Alerting:

| Job         | Tool                     | Description                                                                                                                                    | Installation Location |
| ----------- | ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| Send Alerts | Prometheus Alert Manager | integrate with **Grafana Loki** and **Prometheus** to send alerts through email, ms-teams, telegram, ... when a certain condition/rule is met. | Monitoring Machine    |
