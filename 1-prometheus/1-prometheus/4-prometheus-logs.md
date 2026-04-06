
> **Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
> **Prometheus**: [prometheus-3.10.0.linux-amd64](https://github.com/prometheus/prometheus/releases/download/v3.10.0/prometheus-3.10.0.linux-amd64.tar.gz)

---

[click for more details](https://training.promlabs.com/training/monitoring-and-debugging-prometheus/logs/standard-error-logs/)

---

>**Default location for systemd service logs is in `/var/log/syslog`**

or

```bash
journalctl -xeu prometheus.service
```

---

>**Systemd relocated logs can be found in `/var/log/prometheus/error.log`.**

```bash
tail -f /var/log/prometheus/error.log
```

---

>Log level can be set at run time with `--log.level=<SET_LEVEL>` option.
>
>**Debug Levels:** (`debug` > `info` >  `warn` > `error`).
>
>debug = `debug` + `info` +  `warn` + `error`
>
>info     = `info` +  `warn` + `error`
>
>warn   =  `warn` + `error`
>
>error   = `error`

---
## Relocate prometheus logs:

### 1. Systemd prometheus.service file:

**1.1. Create `/var/log/prometheus/` folder.**

```bash
mkdir /var/log/prometheus/ && chown prometheus:prometheus /var/log/prometheus/
```

**1.2. Edit `prometheus.service` file:**
```bash
vim /etc/systemd/system/prometheus.service
```

>Add the following  under `[Service]` section::

```
# in prometheus only one stream works so we will focus on error.
StandardError=append:/var/log/prometheus/error.log
# StandardOutput=append:/var/log/prometheus/output.log
```

>Note: Set `--log.level=warn` for warnings and errors.

```bash
ExecStart=/usr/local/bin/prometheus \
	--config.file="/etc/prometheus/prometheus.yml" \
	--storage.tsdb.path="/var/lib/prometheus/" \
	--web.enable-lifecycle \
	--web.listen-address=":9090" \
	--log.level=warn
```

---
### 2. Manually run for test:

>Go to prometheus executable folder then run:

```bash
prometheus --config.file="prometheus.yml" --web.enable-lifecycle --web.listen-address=":9090" --log.level=warn  2>> <PATH>/error.log
```

---