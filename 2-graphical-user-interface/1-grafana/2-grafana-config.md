
[Official config web page link](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/)

```bash
vim /etc/grafana/grafana.ini
```

---
### TLS Certificate:

> Check the  [`ssl-tls-self-signed-certificate`](../../ssl-tls-certificate-self-signed.md)  documentation for more info.

```bash
vim /etc/grafana/grafana.ini
```

**Under server section:**
```text
#################################### Server ####################################
[server]
```

**1. remove `;` in `;cert_file =` , `;cert_key =`, `;protocol = http` ->  `cert_file =`, `cert_key =`, `protocol = https`.**

**2. Add the .crt and .key files path:**

>Make sure .crt singing authority is trusted by clients.

```text
protocol = https
cert_file =/etc/grafana/cert/service-signed-public-key.crt
cert_key =/etc/grafana/cert/service-private.key
```

**Run:**
```bash
chown -R grafana:grafana /etc/grafana/
```

```bash
systemctl restart grafana-server.service 
```

---
### Log level:

>Default is `info`.

>in `/etc/grafana/grafana.ini`:

```bash
vim /etc/grafana/grafana.ini
```

**remove `;` in `;level = info` -> `level = <YOU_LEVEL>`.**
 
 **EXP:**
 
```
#################################### Logging ##########################
[log]
# Either "console", "file", "syslog". Default is console and  file
# Use space to separate multiple modes, e.g. "console file"
;mode = console file

# Either "debug", "info", "warn", "error", "critical", default is "info"
level = warn
```

---
### Logs Location:

>in `/etc/grafana/grafana.ini`:

```bash
vim /etc/grafana/grafana.ini
```

```text
[log]
log_file = /var/log/grafana/grafana.log
```

---
### Change web port:

```bash
vim /etc/grafana/grafana.ini
```

**remove `;` in `;http_port = 3000` -> `http_port = <NEW_PORT>`.**

**EXP:**
```
#;http_port = 3000
http_port = <NEW_PORT>
```

---

### Firewall config:

**Allow form source IP to `PORT: 3000` only with protocol(tcp):** **(Recommended)**
```bash
ufw allow from <IP> to any port 3000 proto tcp comment 'blackbox_exporter port'
```

**Open port for everybody:** DANGEROUS!
```bash
ufw allow 3000 comment 'blackbox_exporter port'
```

