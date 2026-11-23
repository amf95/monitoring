> **Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
> **process_exporter:** [process-exporter-0.8.7.linux-amd64](https://github.com/ncabatoff/process-exporter/releases/download/v0.8.7/process-exporter-0.8.7.linux-amd64.tar.gz)

> Port: 9256

---

>**Note:** This tutorial assumes that you already downloaded and extracted `process-exporter-x.y.z.linux-amd64` folder. where `x.y.z` is the version of your choice.

> **NOTE:** Either use `root` user or `sudo`.

---
### 1. Create `process_exporter` user for the service:

>User has no login shell nor home directory.

```bash
useradd --no-create-home --shell /bin/false process_exporter
```

---
### 2. Get in the `process-exporter-x.y.z.linux-amd64` directory:

**EXP:**
```bash
cd process-exporter-0.8.7.linux-amd64
```

`$ ls` Result:
```bash
LICENSE  process_exporter  NOTICE
```

---
### 3. Move `process_exporter` executable to `/usr/local/bin/`:


**3.1. Move `process_exporter` executable to `/usr/local/bin/`:**
```bash
cp process-exporter /usr/local/bin/ 
chown process_exporter:process_exporter /usr/local/bin/process-exporter
```

**3.2. Create directory for `process.yml`:**
```bash
mkdir -P /etc/prometheus/process-exporter-0.8.7.linux-amd64
touch /etc/prometheus/process-exporter-0.8.7.linux-amd64/process.yml
chown -R process_exporter:process_exporter /etc/prometheus/process-exporter-0.8.7.linux-amd64
```

---

>**Hint**: `/usr/local/bin/` is the place where you add custom binaries and executables to be run by all users as any other linux command.

**EXP:**
```bash
process-exporter --config.path=process.yml --web.listen-address=':9256'
```

---
### 4. Create systemd `process_exporter.service`:

**4.1. Create `process_exporter.service` file:**
```bash
vim  /etc/systemd/system/process_exporter.service
```

**4.2. Add the following:**
```bash
[Unit]

Description="process-exporter extracts processes metrics and reports it to prometheus. Default web ui http://localhost:9256/metrics"

Wants=network-online.target

After=network-online.target

# wait x seconds between each time you try to restart on-failure.
StartLimitIntervalSec=30

# try to restart service x times on-failure
StartLimitBurst=3  

[Service]

User=process_exporter

Group=process_exporter

Type=simple

ExecStart=/opt/monitoring/process-exporter-0.8.7.linux-amd64/process-exporter \
	--config.path=/etc/prometheus/process-exporter-0.8.7.linux-amd64/process.yml \
	--web.listen-address=':9256' \
#    --web.config.file="/etc/process_exporter/web-config.yml"

# web-config.yml contains TLS configs.

NoNewPrivileges=true
ProtectSystem=strict
ProtectHome=true

Restart=on-failure

[Install]
WantedBy=multi-user.target
```

> Save and Exit.

**4.3. Reload systemd-daemon and start the service:**

**Reload systemd-daemon:**
```bash
systemctl daemon-reload
```

**Enable service:**
```bash
systemctl enable process_exporter.service
```

**Start service:**
```bash
systemctl start process_exporter.service
```

**Check status:**
```bash
systemctl status process_exporter.service
```

---
### 5. Open service ports in firewall:

**Allow form source IP to `PORT: 9256` only with protocol(tcp):** **(Recommended)**
```bash
ufw allow from <IP> to any port 9256 proto tcp comment 'process_exporter port'
```

**Open port for everybody:** **(DANGEROUS!)**
```bash
ufw allow 9256 comment 'process_exporter port'
```

---
### 6. Make sure its working:

>In any web-browser:

```text
http://<MACHINE_IP>:9256/metrics
```

**EXP:**
```text
http://localhost:9256/metrics
```
