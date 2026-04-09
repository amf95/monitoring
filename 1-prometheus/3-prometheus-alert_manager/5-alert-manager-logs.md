>**Version control:**
>
> **OS**: ubuntu 24.04
>
> **alert-manager**: [alertmanager-0.31.1.linux-amd64.tar.gz](https://github.com/prometheus/alertmanager/releases/download/v0.31.1/alertmanager-0.31.1.linux-amd64.tar.gz)

---

> **NOTE**: It better to turn logs off in `systemd` service file so that they wont take much space.

>**Default location for systemd service logs is in `/var/log/messages`**

or

```bash
journalctl -xeu alertmanager.service
```

---

>**Systemd relocated logs can be found in `/var/log/alertmanager/error.log`.**

```bash
tail -f /var/log/alertmanager/error.log
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
## Relocate alertmanager logs:

### 1. Systemd alertmanager.service file:

**1.1. Create `/var/log/alertmanager/` folder.**

```bash
mkdir /var/log/alertmanager/ && chown alertmanager:alertmanager /var/log/alertmanager/
```

**1.2. Edit `alertmanager.service` file:**
```bash
vim /etc/systemd/system/alertmanager.service
```

>Add the following  under `[Service]` section::

```
# in alertmanager only one stream works so we will focus on error.
StandardError=append:/var/log/alertmanager/error.log
# StandardOutput=append:/var/log/alertmanager/output.log
```

>Note: Set `--log.level=warn` for warnings and errors.

```bash
ExecStart=/usr/local/bin/alertmanager --config.file='/etc/alertmanager/alertmanager.yml' --web.listen-address=':9093' --log.level=warn
```

---
### 2. Manually run for test:

>Go to alertmanager executable folder then run:

```bash
alertmanager --config.file='alertmanager.yml'
```

---