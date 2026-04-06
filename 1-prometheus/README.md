## What is monitoring?
Monitoring is  **collecting, analyzing, and alerting** on predefined **metrics and logs** to track system performance and detect issues.

---
## How everything works together?

![](assets/system-architecture-prometheus.svg)

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
