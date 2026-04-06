
> **Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
> **Prometheus**: [prometheus-3.10.0.linux-amd64](https://github.com/prometheus/prometheus/releases/download/v3.10.0/prometheus-3.10.0.linux-amd64.tar.gz)

---
## Open Config File:

```bash
vim /etc/prometheus/prometheus.yml
```

---
## Check prometheus.yaml file syntax:

```bash
promtool check config /etc/prometheus/prometheus.yml
```

---
## Check rules.yaml file syntax:

```bash
promtool check rules /etc/prometheus/alert_rules.yml
```

---
## Fire Wall Config:

```bash
ufw allow 9090
```

---
## Prometheus Main Config File:

```bash
vim /etc/prometheus/prometheus.yml
```

> **NOTE**: YAML tab is 2 spaces here.

**Basic configs:**
```php
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
           - localhost:9093 # Comment if you don't have prometheus alertmanager.

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  - "rules/*.yml" # create rules directory if it doesn't exist.
  # - "rules/website_1.yml"
  # - "rules/app_1.yml"
  
# create a file for each app for convenience.
scrape_config_files:
  - "targets/*.yml" # create targets directory if it doesn't exist.
  - "targets/app_1/*.yml" # node_exporter.yml, blackbox.yml ...

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]
        labels:
          app: "prometheus"
```

> `scrape_interval`:  How often to scrape targets (default: 15s).
 
> `evaluation_interval`: How often to evaluate rules (check if alert triggers and conditions are met), default: 15s.

> `alertmanager:9093`: uncomment and replace `alertmanager` with your `alertmanager` IP.

> `rule_files`: path to rules that can trigger alert, **EXP**: `rules/app_1.yml`.

> `scrape_config_files`: A list of paths to files containing `scrape_configs` for each target.

> `job_name`: Prometheus saves scraped data based on the `job_name`.

> `static_configs`: contains a list of `targets` which can be machine_domain:PORT name or IP:PORT.

> `targets`: can take a list but usually given one target for convenience.

> `labels`: any user defined key:value, EXP: `app: "prometheus"`, `IP: "10.0.0.25"`.