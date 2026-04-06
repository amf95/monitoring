>**Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
>blackbox: [blackbox_exporter-0.28.0.linux-amd64](https://github.com/prometheus/blackbox_exporter/releases/download/v0.28.0/blackbox_exporter-0.28.0.linux-amd64.tar.gz)

---

>**Default location for systemd service logs is in `/var/log/messages`**

or

```bash
journalctl -xeu blackbox.service
```

---

>**Systemd relocated logs can be found in `/var/log/blackbox/error.log`.**

```bash
tail -f /var/log/blackbox/error.log
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
>error   =`error`

---
## Relocate blackbox logs:

### 1. Systemd blackbox.service file:

**1.1. Create `/var/log/blackbox/` folder.**

```bash
mkdir /var/log/blackbox/ && chown blackbox:blackbox /var/log/blackbox/
```

**1.2. Edit `node_exporter.service` file:**
```bash
vim /etc/systemd/system/blackbox.service
```

>Add the following under `[Service]` section:

```
# in blackbox only one stream works so we will focus on error.
StandardError=append:/var/log/blackbox/error.log
# StandardOutput=append:/var/log/blackbox/output.log
```

>Note: Set `--log.level=warn` for warnings and errors.

```bash
ExecStart=/usr/local/bin/blackbox_exporter \
--config.file='/etc/blackbox/blackbox.yml' \
--web.listen-address=':9115' \
--log.level=warn
```

---
### 2. Manually run for test:

>GNo to node_exporter executable folder then run:

```bash
blackbox_exporter \
--config.file='blackbox.yml' \
--web.listen-address=':9115' \
--log.level=warn 2>> <PATH>/error.log
```

---
