
[Installation guide link](https://www.virtono.com/community/tutorial-how-to/how-to-install-and-configure-prometheus-on-a-linux-server/)

> **Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
> **Prometheus**: [prometheus-3.10.0.linux-amd64](https://github.com/prometheus/prometheus/releases/download/v3.10.0/prometheus-3.10.0.linux-amd64.tar.gz)

---

>**IMPORTANT_NOTE:** This tutorial assumes that you already downloaded and extracted `prometheus-x.y.z.linux-amd64` folder. where `x.y.z` is the version of your choice.

> **NOTE:** Either use `root` user or `sudo`.

---
### 1. Create `prometheus` user for the service:

>User has no login shell nor home directory.

```bash
useradd --no-create-home --shell /bin/false prometheus
```

---
### 2. Get in the `prometheus-x.y.z.linux-amd64` directory:

**EXP:**
```bash
cd prometheus-3.10.0.linux-amd64
```

`$ ls` Result:
```bash
LICENSE  NOTICE  prometheus  prometheus.yml  promtool
```

---
### 3. Create directories and copy executables:

**3.1. Move `prometheus`  `promtool` executables to `/usr/local/bin/`:**
```bash
cp prometheus promtool /usr/local/bin/ \
&& chown prometheus:prometheus /usr/local/bin/prometheus /usr/local/bin/promtool
```

**3.2. Create `/var/log/prometheus/` folder for relocated logs:**
```bash
mkdir /var/log/prometheus/ \
&& chown prometheus:prometheus /var/log/prometheus/
```

**3.3. Create path for database:**
```bash
mkdir -p /var/lib/prometheus \
&& chown -R prometheus:prometheus /var/lib/prometheus
```

**3.4. Create path for configs and move it:**
```bash
mkdir -p /etc/prometheus \
&& touch alert_rules.yml \
&& cp prometheus.yml /etc/prometheus \
&& chown -R prometheus:prometheus /etc/prometheus
```

---

>**Hint**: `/usr/local/bin/` is the place where you add custom binaries and executables to be run by all users as any other linux command.

**EXP:**
```bash
prometheus --config.file="prometheus.yml" --web.enable-lifecycle
```

---
**3.5. *(Optional)* Create `web-config.yml` file(if you are gonna use TLS):**

[web reference link](https://prometheus.io/docs/guides/tls-encryption/)

>Make sure .crt singing authority is trusted by clients.

> Check the [`ssl-tls-self-signed-certificate`](../../ssl-tls-certificate-self-signed.md) documentation for more info.

> **Note:** by default this service doesn't support password encrypted private key.

>**Assuming that you are in the directory where `service-signed-public-key.crt`, and `service-private.key`exist.**

**Create `/etc/prometheus/cert` directory:**
```bash
mkdir -p /etc/prometheus/cert
```

**Copy `service-signed-public-key.crt`, `service-private.key` to `/etc/prometheus/cert`:**
```bash
cp service-signed-public-key.crt service-private.key /etc/prometheus/cert
```

**Create `web-config.yml`:**
```bash
vim /etc/prometheus/web-config.yml
```

**Add the following**:
```yaml
tls_server_config:
  cert_file: /etc/prometheus/cert/service-signed-public-key.crt
  key_file: /etc/prometheus/cert/service-private.key
```

**Change ownership:**
```bash
chown -R prometheus:prometheus /etc/prometheus
```

---
### 4. Create systemd `prometheus.service`:

**4.1. Create `prometheus.service` file:**
```bash
vim  /etc/systemd/system/prometheus.service
```

**4.2. Add the following:**
```bash
[Unit]

Description=Prometheus: service that stores data coming from its exporters

Wants=network-online.target

After=network-online.target

# wait x seconds between each time you try to restart on-failure.
StartLimitIntervalSec=30

# try to restart service x times on-failure
StartLimitBurst=3  

[Service]
User=prometheus
Group=prometheus

Type=simple

ExecStart=/usr/local/bin/prometheus \
	--config.file="/etc/prometheus/prometheus.yml" \
	--storage.tsdb.path="/var/lib/prometheus/" \
	--web.enable-lifecycle \
	--log.level=warn
#	--storage.tsdb.retention.time=7d \
#	--storage.tsdb.retention.size=512MB \
#	--web.config.file=/etc/prometheus/web-config.yml
 
# web-config.yml contains TLS configs.
 
StandardOutput=append:/var/log/prometheus/output.log
StandardError=append:/var/log/prometheus/error.log
	
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

> Save and Exit.

**4.4. Reload systemd-daemon and start the service:**

**Reload systemd-daemon:**
```bash
systemctl daemon-reload
```

**Enable service:**
```bash
systemctl enable prometheus.service
```

**Start service:**
```bash
systemctl start prometheus.service
```

**Check status:**
```bash
systemctl status prometheus.service
```

---
### 7. Open service ports in firewall:

**Allow form source IP to `PORT: 9090` only with protocol(tcp):** **(Recommended)**
```bash
ufw allow from <IP> to any port 9090 proto tcp comment 'prometheus port'
```

**Open port for everybody:** DANGEROUS!
```bash
ufw allow 9090 comment 'prometheus port'
```

---

### 8. Make sure its working:

>In any web-browser:

```text
http://<MACHINE_IP>:9090
```

**EXP:** on the same machine
```text
http://localhost:9090
```

![](assets/Pasted%20image%2020260309132103.png)

![](assets/Pasted%20image%2020260309132144.png)