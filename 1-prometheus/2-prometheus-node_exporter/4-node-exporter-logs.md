> **NOTE**: It better to turn logs off in `systemd` service file so that they wont take much space.

>**Default location for systemd service logs is in `/var/log/syslog`**

or

```bash
journalctl -xeu node_exporter.service
```

---

> **Systemd relocated logs can be found in `/var/log/node_exporter/error.log`.**

```bash
tail -f /var/log/node_exporter/error.log
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
## Relocate node_exporter logs:

### (Option_1) Systemd node_exporter.service file:

**1.1. Create `/var/log/node_exporter/` folder.**

```bash
mkdir /var/log/node_exporter/ && chown node_exporter:node_exporter /var/log/node_exporter/
```

**1.2. Edit `node_exporter.service` file:**
```bash
vim /etc/systemd/system/node_exporter.service
```

>Add the following under `[Service]` section:

```
# in node_exporter only one stream works so we will focus on error.
StandardError=append:/var/log/node_exporter/error.log
# StandardOutput=append:/var/log/node_exporter/output.log
```

>Note: Set `--log.level=warn` for warnings and errors.

```bash
ExecStart=/usr/local/bin/node_exporter \
		--collector.systemd \
        --web.listen-address=':9100' \
        --log.level=warn
```

---
### (Option_2) Manually run for test:

>GNo to node_exporter executable folder then run:

```bash
node_exporter --collector.systemd --web.listen-address=':9100' --log.level=warn  2>> <PATH>/error.log
```

---