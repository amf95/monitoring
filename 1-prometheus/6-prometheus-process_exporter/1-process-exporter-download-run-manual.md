
> **Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
> **process_exporter:** [process-exporter-0.8.7.linux-amd64](https://github.com/ncabatoff/process-exporter/releases/download/v0.8.7/process-exporter-0.8.7.linux-amd64.tar.gz)

> Port: 9256

---
### 1. Download `process_exporter`:

**1.1. Go to this link:**
[https://github.com/prometheus/process-exporter/releases/](https://github.com/ncabatoff/process-exporter/releases)

**1.2. Scroll down, click on `Show all` then right click on the `linux-amd64` release.**

**1.3. Copy the download link.**

**1.4. In your linux machine terminal run:**

```bash
wget <LINK_YOU_JUST_COPIED>
```

**EXP:**
```bash
wget https://github.com/ncabatoff/process-exporter/releases/download/v0.8.7/process-exporter-0.8.7.linux-amd64.tar.gz
```

**`$ ls` Result:**
```bash
process-exporter-0.8.7.linux-amd64.tar.gz
```

> Size is about `11M` .
 
---
### 2. Extract downloaded  `process-exporter-x.y.z.linux-amd64.tar.gz` file:

```bash
tar -xvf <FILE_YOU_JUST_DOWNLOADED>
```

**EXP:**
```bash
tar -xvf process-exporter-0.8.7.linux-amd64.tar.gz
```

**`$ ls` Result:**
```bash
process-exporter-0.8.7.linux-amd64 process-exporter-0.8.7.linux-amd64.tar.gz
```

---
### 3. Get in the `process_exporter-x.y.z.linux-amd64` directory:

**EXP:**
```bash
cd process-exporter-0.8.7.linux-amd64
```

`$ ls` Result:
```bash
LICENSE  process-exporter  NOTICE
```

---
### 4. (Optional) Open service ports in firewall:

**Allow form source IP to `PORT: 9256` only with protocol(tcp):** **(Recommended)**
```bash
ufw allow from <IP> to any port 9256 proto tcp comment 'process_exporter port'
```

**Open port for everybody:** **(DANGEROUS!)**
```bash
ufw allow 9256 comment 'process_exporter port'
```

---
### 5.Run `process-exporter` executable:

```bash
process-exporter --config.path=process.yml --web.listen-address=':9256'
```

> `--config.path=process.yml`: `process.yml` is the file that contains the names of the processes that you want to monitor.

>`--web.listen-address=':9256'` `9256` is the port that prometheus will request and the web metrics UI will use.

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

---