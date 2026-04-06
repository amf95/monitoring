
[Installation guide link](https://devopscube.com/set-up-blackbox-exporter-on-vm/)

>**Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
>blackbox: [blackbox_exporter-0.28.0.linux-amd64](https://github.com/prometheus/blackbox_exporter/releases/download/v0.28.0/blackbox_exporter-0.28.0.linux-amd64.tar.gz)

---
### 1. Download `blackbox_exporter`:

**1.1. Go to this link:**
https://github.com/prometheus/blackbox_exporter/releases

[Click to download blackbox_exporter-0.28.0.linux-amd64](https://github.com/prometheus/blackbox_exporter/releases/download/v0.28.0/blackbox_exporter-0.28.0.linux-amd64.tar.gz)

**1.2. Scroll down, click on `Show all` then right click on the `linux-amd64` release.**

**1.3. Copy the download link.**

**1.4. In your linux machine terminal run:**

```bash
wget <LINK_YOU_JUST_COPIED>
```

**EXP:**
```bash
wget https://github.com/prometheus/blackbox_exporter/releases/download/v0.25.0/blackbox_exporter-0.28.0.linux-amd64.tar.gz
```

**`$ ls` Result:**
```bash
blackbox_exporter-0.28.0.linux-amd64.tar.gz
```

---

### 2. Extract downloaded  `blackbox_exporter-x.y.z.linux-amd64.tar.gz` file:

```bash
tar -xvf <FILE_YOU_JUST_DOWNLOADED>
```

**EXP:**
```bash
tar -xvf blackbox_exporter-0.28.0.linux-amd64.tar.gz
```

**`$ ls` Result:**
```bash
blackbox_exporter-0.28.0.linux-amd64 blackbox_exporter-0.28.0.linux-amd64.tar.gz
```

---

### 3. Get in the`blackbox_exporter-x.y.z.linux-amd64` directory:

**EXP:**
```bash
cd ~/blackbox_exporter-0.28.0.linux-amd64/
```

`$ ls` Result:
```bash
blackbox_exporter  blackbox.yml  LICENSE  NOTICE
```

---
### 4. Open service ports in firewall:

**Allow form source IP to `PORT: 9090` only with protocol(tcp):** **(Recommended)**
```bash
ufw allow from <IP> to any port 9115 proto tcp comment 'blackbox_exporter port'
```

**Open port for everybody:** DANGEROUS!
```bash
ufw allow 9115 comment 'blackbox_exporter port'
```

---
### 5.Run `blackbox_exporter` executable:

> Make sure you are in the same folder of the `blackbox_exporter` executable.

> If it doesn't run -> run: `chmod +x blackbox_exporter`

```bash
blackbox_exporter --config.file='blackbox.yml' --web.listen-address=':9115'
```

>`--config.file`: full path to `blackbox.yml`.
>
>`--web.listen-address` tells it which port `prometheus` will request and the web metrics UI will use.

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
