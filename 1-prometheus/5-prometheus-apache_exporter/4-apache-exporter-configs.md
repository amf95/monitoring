
> **Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
> **apache_exporter:** [apache_exporter.linux-amd64](https://github.com/Lusitaniae/apache_exporter/releases/download/v1.1.1/apache_exporter-1.1.1.linux-amd64.tar.gz)

---

[Config Options Source](https://github.com/prometheus/apache_exporter)

[Enable Systemd Monitoring](https://stackoverflow.com/questions/53290611/where-can-i-find-the-list-of-systemd-node-exporter-metrics)

> No direct config file for this service.

>Configuration can be done during running node-exporter.

---
## Enable Systemd Scraping:

```bash
ExecStart=/usr/local/bin/apache_exporter \
	--scrape_uri="http://localhost/server-status?auto" \
	--telemetry.endpoint="/metrics" \
	--web.listen-address=":9117"
#    --web.config.file="/etc/apache_exporter/web-config.yml"
```

---
## FireWall Config:

**To everybody:**
```bash
ufw allow 9117
```

or 

**To Prometheus machine:**
```bash
ufw allow from <IP> to any port 9117 comment 'prometheus machine'
```

> `<IP>`: Prometheus machine IP.

---

