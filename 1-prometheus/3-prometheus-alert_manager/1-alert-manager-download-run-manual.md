
[Installion guide link](https://developer.couchbase.com/tutorial-configure-alertmanager)

>**Version control:**
>
> **OS**: `ubuntu 24.04`
>
>**alert-manager**: [alertmanager-0.31.1.linux-amd64.tar.gz](https://github.com/prometheus/alertmanager/releases/download/v0.31.1/alertmanager-0.31.1.linux-amd64.tar.gz)

---
### 1. Download `alertmanager`:

[Click to download lertmanager-0.31.1.linux-amd64.tar.gz directly](https://github.com/prometheus/alertmanager/releases/download/v0.31.1/alertmanager-0.31.1.linux-amd64.tar.gz)

**1.1. Go to this link:**
https://github.com/prometheus/alertmanager/releases

**1.2. Scroll down, click on `Show all` then right click on the `linux-amd64` release.**

**1.3. Copy the download link.**

**1.4. In your linux machine terminal run:**

```bash
wget <LINK_YOU_JUST_COPIED>
```

**EXP:**
```bash
wget https://github.com/prometheus/alertmanager/releases/download/v0.31.1/alertmanager-0.31.1.linux-amd64.tar.gz
```

**`$ ls` Result:**
```bash
alertmanager-0.31.1.linux-amd64.tar.gz
```

> `alertmanager-0.31.1.linux-amd64.tar.gz` size is about  `35.9MB`.

---
### 2. Extract downloaded  `alertmanager-x.y.z.linux-amd64.tar.gz` file:

```bash
tar -xvf <FILE_YOU_JUST_DOWNLOADED>
```

**EXP:**
```bash
alertmanager-0.31.1.linux-amd64.tar.gz
```

**`$ ls` Result:**
```bash
alertmanager-0.31.1.linux-amd64 alertmanager-0.31.1.linux-amd64.tar.gz
```

---
### 3. Get in the `alertmanager-x.y.z.linux-amd64` directory:

**EXP:**
```bash
cd ~/alertmanager-0.31.1.linux-amd64
```

`$ ls` Result:
```bash
alertmanager  alertmanager.yml  amtool  LICENSE  NOTICE
```

---
### 4. Open service ports in firewall:

**Allow form source IP to `PORT: 9090` only with protocol(tcp):** **(Recommended)**
```bash
ufw allow from <IP> to any port 9093 proto tcp comment 'alertmanager port'
```

**Open port for everybody:** DANGEROUS!
```bash
ufw allow 9093 comment 'alertmanager port'
```


---
### 5. Run prometheus executable:

> Make sure you are in the same folder of the `alertmanager` executable.

> If it doesn't run -> run: `chmod +x alertmanager`

```bash
alertmanager --config.file='alertmanager.yml'
```

> `--config.file`: full path to `alertmanager.yml`.

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
