
> **Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
> **apache_exporter:** [apache_exporter.linux-amd64](https://github.com/Lusitaniae/apache_exporter/releases/download/v1.1.1/apache_exporter-1.1.1.linux-amd64.tar.gz)

---
### 1. Download `apache_exporter`:

**1.1. Go to this link:**
https://github.com/Lusitaniae/apache_exporter/releases

**1.2. Scroll down, click on `Show all` then right click on the `linux-amd64` release.**

**1.3. Copy the download link.**

**1.4. In your linux machine terminal run:**

```bash
wget <LINK_YOU_JUST_COPIED>
```

**EXP:**
```bash
wget https://github.com/Lusitaniae/apache_exporter/releases/download/v1.1.1/apache_exporter-1.1.1.linux-amd64.tar.gz
```

**`$ ls` Result:**
```bash
apache_exporter-1.1.1.linux-amd64.tar.gz
```

> Size is about `11M` .
 
---
### 2. Extract downloaded  `apache_exporter-x.y.z.linux-amd64.tar.gz` file:

```bash
tar -xvf <FILE_YOU_JUST_DOWNLOADED>
```

**EXP:**
```bash
tar -xvf apache_exporter-1.1.1.linux-amd64.tar.gz
```

**`$ ls` Result:**
```bash
apache_exporter-1.1.1.linux-amd64 apache_exporter-1.1.1.linux-amd64.tar.gz
```

---
### 3. Get in the `apache_exporter-x.y.z.linux-amd64` directory:

**EXP:**
```bash
cd apache_exporter-1.1.1.linux-amd64
```

`$ ls` Result:
```bash
apache_exporter  LICENSE
```

---
### 4. (Optional) Open service ports in firewall:

**Allow form source IP to `PORT: 9117` only with protocol(tcp):** **(Recommended)**
```bash
ufw allow from <IP> to any port 9117 proto tcp comment 'apache_exporter port'
```

**Open port for everybody:** DANGEROUS!
```bash
ufw allow 9117 comment 'apache_exporter port'
```

---
### 5.Run `apache_exporter` executable:

```bash
apache_exporter --scrape_uri="http://localhost/server-status?auto" --telemetry.endpoint="/metrics" --web.listen-address=":9117"
```

> `--scrape_uri="http://localhost/server-status?auto"`: Requests data from apache server(localhost) status when you enable `a2enmod status`.

> `--telemetry.endpoint="/metrics"`: The endpoint prometheus will scrap.

>`--web.listen-address=':9117'` tells it which port prometheus will request and the web metrics UI will use.

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

---