
[Installation guide link](https://devopscube.com/set-up-blackbox-exporter-on-vm/)

>**Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
>blackbox: [blackbox_exporter-0.28.0.linux-amd64](https://github.com/prometheus/blackbox_exporter/releases/download/v0.28.0/blackbox_exporter-0.28.0.linux-amd64.tar.gz)

---

>**IMPORTANT_NOTE:** This tutorial assumes that you already downloaded and extracted `blackbox_exporter-x.y.z.linux-amd64` folder. where `x.y.z` is the version of your choice.

> **NOTE:** Either use `root` user or `sudo`.

---
### 1. Create `blackbox` user for the service:

>User has no login shell nor home directory.

```bash
useradd --no-create-home --shell /bin/false blackbox
```

---
### 2. Get in the `blackbox_exporter-x.y.z.linux-amd64` directory:

**EXP:**
```bash
cd blackbox_exporter-0.28.0.linux-amd64
```

`$ ls` Result:
```bash
blackbox_exporter  blackbox.yml  LICENSE  NOTICE
```

---
### 3. Create directories and copy executables:

**3.1. Move `blackbox_exporter` executable to `/usr/local/bin/`:**
```bash
cp blackbox_exporter /usr/local/bin/ \
&& chown blackbox:blackbox /usr/local/bin/node_exporter
```

**3.2. Create path for configs and move it:**
```bash
mkdir -p /etc/blackbox \
&& cp blackbox.yml /etc/blackbox \
&& chown -R blackbox:blackbox /etc/blackbox
```

**3.3. Create `/var/log/blackbox/` folder for relocated logs.**
```bash
mkdir /var/log/blackbox/ && chown blackbox:blackbox /var/log/blackbox/
```

---

>**Hint**: `/usr/local/bin/` is the place where you add custom binaries and executables to be run by all users as any other linux command.

**EXP:**
```bash
blackbox_exporter --config.file='/etc/blackbox/blackbox.yml' --web.listen-address=':9115'
```

---

**3.4. *(Optional)* Create `web-config.yml` file(if you are gonna use TLS):**

[web reference link](https://medium.com/@abdullah.eid.2604/prometheus-node-exporter-security-9118f65a9f59)

>Make sure .crt singing authority is trusted by clients.

> Check the  [`ssl-tls-self-signed-certificate`](../../ssl-tls-certificate-self-signed.md)  documentation for more info.

> **Note:** by default this service doesn't support password encrypted private key.

>**Assuming that you are in the directory where `service-signed-public-key.crt`, and `service-private.key`exist.**

**Create `/etc/blackbox/cert` directory:**
```bash
mkdir -p /etc/blackbox/cert
```

**Copy `service-signed-public-key.crt`, `service-private.key` to `/etc/blackbox/cert`:**
```bash
cp service-signed-public-key.crt service-private.key /etc/blackbox/cert
```

**Create `web-config.yml`:**
```bash
vim /etc/blackbox/web-config.yml
```

**Add the following**:
```yaml
tls_server_config:
  cert_file: /etc/blackbox/cert/service-signed-public-key.crt
  key_file: /etc/blackbox/cert/service-private.key
```

**Change ownership:**
```bash
chown -R blackbox:blackbox /etc/blackbox
```

---
### 4. Create systemd `blackbox.service`:

**4.2. Create `blackbox.service` file:**
```bash
vim  /etc/systemd/system/blackbox.service
```

**4.3. Add the following:**
```bash
[Unit]

Description=Blackbox Exporter Service

Wants=network-online.target

After=network-online.target

# wait x seconds between each time you try to restart on-failure.
StartLimitIntervalSec=30

# try to restart service x times on-failure
StartLimitBurst=3  

[Service]
User=blackbox
Group=blackbox

Type=simple

ExecStart=/usr/local/bin/blackbox_exporter \
	--config.file='/etc/blackbox/blackbox.yml' \
	--web.listen-address=':9115' \
	--log.level=info
#	--web.config.file=/etc/blackbox/web-config.yml

# web-config.yml contains TLS configs.

StandardError=append:/var/log/blackbox/error.log

Restart=on-failure

[Install]
WantedBy=multi-user.target
```

> Save and Exit.

**4.5. Reload systemd-daemon and start the service:**

**Reload systemd-daemon:**
```bash
systemctl daemon-reload
```

**Enable service:**
```bash
systemctl enable blackbox.service
```

**Start service:**
```bash
systemctl start blackbox.service
```

**Check status:**
```bash
systemctl status blackbox.service
```

---
### 5. Open service ports in firewall:

**Allow form source IP to `PORT: 9115` only with protocol(tcp):** **(Recommended)**
```bash
ufw allow from <IP> to any port 9115 proto tcp comment 'blackbox_exporter port'
```

**Open port for everybody:** DANGEROUS!
```bash
ufw allow 9115 comment 'prometheus port'
```

---
### 6. Make sure its working:

>In any web-browser:

```text
http://<MACHINE_IP>:9115/metrics
```

**EXP:**
```text
http://localhost:9115/metrics
```

---
