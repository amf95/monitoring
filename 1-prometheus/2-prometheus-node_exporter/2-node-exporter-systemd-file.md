
[Installation guide link](https://docs.vmware.com/en/VMware-vRealize-Operations-Management-Pack-for-Kubernetes/1.8.1/management-pack-for-kubernetes/GUID-A1B68BE5-EF38-48E1-AA80-FD71E6F19989.html)

> **Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
> **node_exporter:** [node_exporter-1.8.2.linux-amd64](https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz)

---
>**Note:** This tutorial assumes that you already downloaded and extracted `node_exporter-X.X.X.linux-amd64` folder. where `X.X.X` is the version of your choice.
---
### 1. Create `node_exporter` user for the service:

>User has no login shell nor home directory.

```bash
useradd --no-create-home --shell /bin/false node_exporter
```

---
### 2. Get in the `node_exporter-X.X.X.linux-amd64` directory:

**EXP:**
```bash
cd ~/node_exporter-1.8.2.linux-amd64/
```

`$ ls` Result:
```bash
LICENSE  node_exporter  NOTICE
```

---
### 3. Move `node_exporter` executable to `/usr/local/bin/`:


**3.1. Move `node_exporter` executable to `/usr/local/bin/`:**
```bash
cp node_exporter /usr/local/bin/ \
&& chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

---

>**Hint**: `/usr/local/bin/` is the place where you add custom binaries and executables to be run by all users as any other linux command.

**EXP:**
```bash
node_exporter --collector.systemd --web.listen-address=':9100'
```

---
**3.3. (Optional) Create `web-config.yml` file(if you are gonna use TLS):**

[web reference link](https://medium.com/@abdullah.eid.2604/prometheus-node-exporter-security-9118f65a9f59)

>Make sure .crt singing authority is trusted by clients.

> Check the  [`ssl-tls-self-signed-certificate`](../../ssl-tls-certificate-self-signed.md)  documentation for more info.

> **Note:** by default this service doesn't support password encrypted private key.

>**Assuming that you are in the directory where `service-signed-public-key.crt`, and `service-private.key`exist.**

**Create `/etc/node_exporter/cert` directory:**
```bash
mkdir -p /etc/node_exporter/cert
```

**Copy `service-signed-public-key.crt`, `service-private.key` to `/etc/node_exporter/cert`:**
```bash
cp service-signed-public-key.crt service-private.key /etc/node_exporter/cert
```

**Create `web-config.yml`:**
```bash
vim /etc/node_exporter/web-config.yml
```

**Add the following**:
```yaml
tls_server_config:
  cert_file: /etc/node_exporter/cert/service-signed-public-key.crt
  key_file: /etc/node_exporter/cert/service-private.key
```

**Change ownership:**
```bash
chown -R node_exporter:node_exporter /etc/node_exporter
```

---
### 4. Create systemd `node_exporter.service`:

**4.1. Create `/var/log/node_exporter/` folder for relocated logs.**

```bash
mkdir /var/log/node_exporter/ && chown node_exporter:node_exporter /var/log/node_exporter/
```

**4.2. Create `node_exporter.service` file:**
```bash
vim  /etc/systemd/system/node_exporter.service
```

**4.3. Add the following:**

> **Note: if you are gonna use TLS modify `ExecStart` as follows:**
```bash
ExecStart=/usr/local/bin/node_exporter_exporter --config.file='/etc/node_exporter/node_exporter.yml' --web.listen-address=':9100' --log.level=warn --web.config.file="/etc/node_exporter/web-config.yml"
```

```bash
[Unit]

Description="node_exporter extracts system metrics and reports it to prometheus. Default web ui http://localhost:9100/metrics"

Wants=network-online.target

After=network-online.target

# wait x seconds between each time you try to restart on-failure.
StartLimitIntervalSec=30

# try to restart service x times on-failure
StartLimitBurst=3  

[Service]

User=node_exporter

Group=node_exporter

Type=simple

ExecStart=/usr/local/bin/node_exporter \
#	--collector.systemd \
	--web.listen-address=':9100' \
	--log.level=warn

Restart=on-failure

# node-exporter only outputs one file for all logs(info, warn, error,...). 
StandardError=append:/var/log/node_exporter/error.log

[Install]
WantedBy=multi-user.target
```

**4.3. Reload systemd-daemon and start the service:**

**Reload systemd-daemon:**
```bash
systemctl daemon-reload
```

**Enable service:**
```bash
systemctl enable node_exporter.service
```

**Start service:**
```bash
systemctl start node_exporter.service
```

**Check status:**
```bash
systemctl status node_exporter.service
```

---
### 5. Open service ports in firewall:
```bash
ufw allow 9100
```

---
### 6. Make sure its working:

>In any web-browser:

```text
http://<MACHINE_IP>:9100/metrics
```

**EXP:**
```text
http://localhost:9100/metrics
```
