
[Installation guide link](https://docs.vmware.com/en/VMware-vRealize-Operations-Management-Pack-for-Kubernetes/1.8.1/management-pack-for-kubernetes/GUID-A1B68BE5-EF38-48E1-AA80-FD71E6F19989.html)

> **Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
> **node_exporter:** [node_exporter-1.8.2.linux-amd64](https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz)

---
### 1. Download `node_exporter`:

**1.1. Go to this link:**
https://github.com/prometheus/node_exporter/releases/

**1.2. Scroll down, click on `Show all` then right click on the `linux-amd64` release.**

**1.3. Copy the download link.**

**1.4. In your linux machine terminal run:**

```bash
wget <LINK_YOU_JUST_COPIED>
```

**EXP:**
```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz
```

**`$ ls` Result:**
```bash
node_exporter-1.8.2.linux-amd64.tar.gz
```

> Size is about `11M` .
 
---
### 2. Extract downloaded  `node_exporter-X.X.X.linux-amd64.tar.gz` file:

```bash
tar -xvf <FILE_YOU_JUST_DOWNLOADED>
```

**EXP:**
```bash
tar -xvf node_exporter-1.8.2.linux-amd64.tar.gz
```

**`$ ls` Result:**
```bash
node_exporter-1.8.2.linux-amd64 node_exporter-1.8.2.linux-amd64.tar.gz
```

---
### 3. Get in the `node_exporter-X.X.X.linux-amd64` directory:

**EXP:**
```bash
cd ~/node_exporter-1.8.2.linux-amd64/
```

`$ ls` Result:
```bash
LICENSE  node_exporter  NOTICE
```

---
### 4. (Optional) Open service ports in firewall:
```bash
ufw allow 9100
```

---
### 5.Run `node_exporter` executable:

```bash
node_exporter --collector.systemd --web.listen-address=':9100'
```

>`--collector.systemd` forces it to collect systemd and services status.
>
>`--web.listen-address=':9100'` tells it which port prometheus will request and the web metrics UI will use.

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

---