> **Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
> **process_exporter:** [process-exporter-0.8.7.linux-amd64](https://github.com/ncabatoff/process-exporter/releases/download/v0.8.7/process-exporter-0.8.7.linux-amd64.tar.gz)

> Port: 9256

---

```bash
vim /etc/prometheus/process-exporter-0.8.7.linux-amd64/process.yml
```

```bash
process_names:
  - name: "{{.Comm}}"
    comm:
      - apache2
      - mariadb
      - redis
      - php8.4-fpm
```

---
## FireWall Config:

**Allow form source IP to `PORT: 9256` only with protocol(tcp):** **(Recommended)**
```bash
ufw allow from <IP> to any port 9256 proto tcp comment 'process_exporter port'
```

**Open port for everybody:** **(DANGEROUS!)**
```bash
ufw allow 9256 comment 'process_exporter port'
```


