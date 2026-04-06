
[Installion guide link 1](https://developer.couchbase.com/tutorial-configure-alertmanager)
[Installion guide link 2](https://gist.github.com/Tumar/cd1ef5ecd88086d49d04d9a4205f4763)

---

>**Version control:**
>
> **OS**: ubuntu 24.04
>
> **alert-manager**: [alertmanager-0.31.1.linux-amd64.tar.gz](https://github.com/prometheus/alertmanager/releases/download/v0.31.1/alertmanager-0.31.1.linux-amd64.tar.gz)

---

>**IMPORTANT_NOTE:** This tutorial assumes that you already downloaded and extracted `alertmanager-x.y.z.linux-amd64` folder. where `x.y.z` is the version of your choice.

> **NOTE:** Either use `root` user or `sudo`.

---
### 1. Create `alertmanager` user for the service:

>User has no login shell nor home directory.

```bash
useradd --no-create-home --shell /bin/false alertmanager
```

---
### 2. Get in the `alertmanager-x.y.z.linux-amd64` directory:

**EXP:**
```bash
cd alertmanager-0.31.1.linux-amd64
```

`$ ls` Result:
```bash
alertmanager  alertmanager.yml  amtool  LICENSE  NOTICE
```

---
### 3. Create directories and copy executables:

 **3.1. Move `alertmanager`  `amtool` executables to `/usr/local/bin/`:**
```bash
cp alertmanager amtool /usr/local/bin/ \
&& chown alertmanager:alertmanager /usr/local/bin/alertmanager /usr/local/bin/amtool
```

**3.2. Create `/var/log/alertmanager/` folder for relocated logs.**

```bash
mkdir /var/log/alertmanager/ && chown -R alertmanager:alertmanager /var/log/alertmanager/
```

**3.3. Create path for configs and move it:**
```bash
mkdir -p /etc/alertmanager \
&& cp alertmanager.yml /etc/alertmanager \
&& chown -R alertmanager:alertmanager /etc/alertmanager
```

**3.4. Create path for data:**
```bash
mkdir -p /etc/alertmanager/data \
&& chown -R alertmanager:alertmanager /etc/alertmanager/data
```

---

>**Hint**: `/usr/local/bin/` is the place where you add custom binaries and executables to be run by all users as any other linux command.

**EXP:**
```bash
alertmanager --config.file='alertmanager.yml'
```

---
### 4. Create systemd `alertmanager.service`:

**4.1. Create `alertmanager.service` file:**
```bash
vim  /etc/systemd/system/alertmanager.service
```

**4.2. Add the following:**
```bash
[Unit]

Description=Prometheus Alert Manager Service

Wants=network-online.target

After=network-online.target

# wait x seconds between each time you try to restart on-failure.
StartLimitIntervalSec=30

# try to restart service x times on-failure
StartLimitBurst=3  

[Service]
User=alertmanager
Group=alertmanager

Type=simple

ExecStart=/usr/local/bin/alertmanager --config.file='/etc/alertmanager/alertmanager.yml' \
--web.listen-address=':9093' \
--log.level=warn \
--storage.path=/etc/alertmanager/data

StandardError=append:/var/log/alertmanager/error.log

Restart=on-failure

[Install]
WantedBy=multi-user.target
```

**4.4. Reload systemd-daemon and start the service:**

**Reload systemd-daemon:**
```bash
systemctl daemon-reload
```

**Enable service:**
```bash
systemctl enable alertmanager.service
```

**Start service:**
```bash
systemctl start alertmanager.service
```

**Check status:**
```bash
systemctl status alertmanager.service
```

---
### 5. Open service ports in firewall:

**Allow form source IP to `PORT: 9093` only with protocol(tcp):** **(Recommended)**
```bash
ufw allow from <IP> to any port 9093 proto tcp comment 'alertmanager port'
```

**Open port for everybody:** DANGEROUS!
```bash
ufw allow 9093 comment 'alertmanager port'
```

---

### 6. Make sure its working:

>In any web-browser:

```text
http://<MACHINE_IP>:9093
```

**EXP:**
```text
http://localhost:9093
```

---
