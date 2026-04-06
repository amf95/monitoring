
[Installation guide link ](https://www.virtono.com/community/tutorial-how-to/how-to-install-and-configure-prometheus-on-a-linux-server/)

> **Version control:**
>
> **OS**: `Ubuntu 24.04`
>
> **Prometheus**: [prometheus-3.10.0.linux-amd64](https://github.com/prometheus/prometheus/releases/download/v3.10.0/prometheus-3.10.0.linux-amd64.tar.gz)

---
### 1. Download `prometheus`:

**[Click to download prometheus-3.10.0.linux-amd64.tar.gz directly](https://github.com/prometheus/prometheus/releases/download/v3.10.0/prometheus-3.10.0.linux-amd64.tar.gz)**

**1.1. Go to this link:** https://github.com/prometheus/prometheus/releases

**1.2. Scroll down, click on `Show all` then right click on the `linux-amd64` release.**

**1.3. Copy the download link.**

**1.4. In your linux machine terminal run:**

```bash
wget <LINK_YOU_JUST_COPIED>
```

**EXP:**
```bash
wget https://github.com/prometheus/prometheus/releases/download/v3.10.0/prometheus-3.10.0.linux-amd64.tar.gz
```

**`$ ls` Result:**
```bash
prometheus-3.10.0.linux-amd64.tar.gz
```

> `prometheus-3.10.0.linux-amd64.tar.gz` size is about  `135MB`.

---
### 2. Extract downloaded  `prometheus-x.y.z.linux-amd64.tar.gz` file:

> **Note**: tar might not work if your CPU is not enough.

```bash
tar -xvf <FILE_YOU_JUST_DOWNLOADED>
```

**EXP:**
```bash
tar -xvf prometheus-3.10.0.linux-amd64.tar.gz
```

**`$ ls` Result:**
```bash
prometheus-3.10.0.linux-amd64 prometheus-3.10.0.linux-amd64.tar.gz
```

---
### 3. Get in the `prometheus-x.y.z.linux-amd64` directory:

**EXP:**
```bash
cd prometheus-3.10.0.linux-amd64
```

`$ ls` Result:
```bash
LICENSE  NOTICE  prometheus  prometheus.yml  promtool
```

---
### 4. Open service ports in firewall:

**Allow form source IP to `PORT: 9090` only with protocol(tcp):** **(Recommended)**
```bash
ufw allow from <IP> to any port 9090 proto tcp comment 'prometheus port'
```

**Open port for everybody:** DANGEROUS!
```bash
ufw allow 9090 comment 'prometheus port'
```

---
### 5. Run `prometheus` executable for testing:

> Make sure you are in the same folder of the `prometheus` executable.

> If it doesn't run -> run: `chmod +x prometheus`

```bash
prometheus --config.file="prometheus.yml" --web.enable-lifecycle
```

> `--config.file`: full path to `prometheus.yml`.

> `--web.enable-lifecycle`: enables HTTP endpoints that let you manage Prometheus at runtime without restarting it.

---
### 6. Make sure its working:

**In any web-browser:**
```text
http://<MACHINE_IP>:9090
```

**EXP:** on the same machine.
```text
http://localhost:9090
```

---

**http://localhost:9090/query:**

![](assets/Pasted%20image%2020260309132103.png)

**http://localhost:9090/targets:**

![](assets/Pasted%20image%2020260309132144.png)

---