> **Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
> **node_exporter:** [node_exporter-1.11.1.linux-amd64](https://github.com/prometheus/node_exporter/releases/download/v1.11.1/node_exporter-1.11.1.linux-amd64.tar.gz)

---

[Config Options Source](https://github.com/prometheus/node_exporter)

[Enable Systemd Monitoring](https://stackoverflow.com/questions/53290611/where-can-i-find-the-list-of-systemd-node-exporter-metrics)

> No direct config file for this service.

>Configuration can be done during running node-exporter.

---
## Enable Systemd Scraping:

```bash
ExecStart=/usr/local/bin/node_exporter \
		--collector.systemd \
        --web.listen-address=':9100'
```

---
## FireWall Config:

**To everybody:**
```bash
ufw allow 9100
```

or 

**To Prometheus machine:**
```bash
ufw allow from <IP> to any port 9100 comment 'prometheus machine'
```

> `<IP>`: Prometheus machine IP.

---

![](assets/Pasted%20image%2020260310110829.png)