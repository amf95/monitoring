
> **Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
> **apache_exporter:** [apache_exporter-1.1.1.linux-amd64](https://github.com/Lusitaniae/apache_exporter/releases/download/v1.1.1/apache_exporter-1.1.1.linux-amd64.tar.gz)

---

>**Note:** This tutorial assumes that you already downloaded and extracted `apache_exporter-x.y.z.linux-amd64` folder. where `x.y.z` is the version of your choice.

> **NOTE:** Either use `root` user or `sudo`.

---
### 1. Create `apache_exporter` user for the service:

>User has no login shell nor home directory.

```bash
useradd --no-create-home --shell /bin/false apache_exporter
```

---
### 2. Get in the `apache_exporter-x.y.z.linux-amd64` directory:

**EXP:**
```bash
cd apache_exporter-1.1.1.linux-amd64
```

`$ ls` Result:
```bash
apache_exporter  LICENSE
```

---
### 3. Move `apache_exporter` executable to `/usr/local/bin/`:


**3.1. Move `apache_exporter` executable to `/usr/local/bin/`:**
```bash
cp apache_exporter /usr/local/bin/
chown apache_exporter:apache_exporter /usr/local/bin/apache_exporter
```

**3.2. Create `/var/log/apache_exporter/` folder for relocated logs.**
```bash
mkdir /var/log/apache_exporter/
chown apache_exporter:apache_exporter /var/log/apache_exporter
```

---

>**Hint**: `/usr/local/bin/` is the place where you add custom binaries and executables to be run by all users as any other linux command.

**EXP:**
```bash
apache_exporter --scrape_uri="http://localhost/server-status?auto" --telemetry.endpoint="/metrics" --web.listen-address=":9117"
```

---
### 4. Create systemd `apache_exporter.service`:

**4.1. Create `apache_exporter.service` file:**
```bash
vim  /etc/systemd/system/apache_exporter.service
```

> Add `--insecure` option to with https.

**4.2. Add the following:**
```bash
[Unit]

Description="Prometheus Apache Exporter"

Wants=network-online.target

After=network-online.target apache2.service httpd.service

# wait x seconds between each time you try to restart on-failure.
StartLimitIntervalSec=30

# try to restart service x times on-failure
StartLimitBurst=3  

[Service]

User=apache_exporter

Group=apache_exporter

Type=simple

ExecStart=/usr/local/bin/apache_exporter \
#	--scrape_uri="https://localhost/server-status?auto" \
#	--insecure
	--scrape_uri="http://localhost/server-status?auto" \
	--telemetry.endpoint="/metrics" \
	--web.listen-address=":9117" \


#    --web.config.file="/etc/apache_exporter/web-config.yml"

# web-config.yml contains TLS configs.

NoNewPrivileges=true
ProtectSystem=strict
ProtectHome=true

Restart=on-failure

# node-exporter only outputs one file for all logs(info, warn, error,...). 
StandardError=append:/var/log/apache_exporter/error.log

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
systemctl enable apache_exporter.service
```

**Start service:**
```bash
systemctl start apache_exporter.service
```

**Check status:**
```bash
systemctl status apache_exporter.service
```

---
### 5. Open service ports in firewall:

**Allow form source IP to `PORT: 9117` only with protocol(tcp):** **(Recommended)**
```bash
ufw allow from <IP> to any port 9117 proto tcp comment 'apache_exporter port'
```

**Open port for everybody:** DANGEROUS!
```bash
ufw allow 9117 comment 'apache_exporter port'
```

---
### 6. Make sure its working:

>In any web-browser:

```text
http://<MACHINE_IP>:9117/metrics
```

**EXP:**
```text
http://localhost:9117/metrics
```
