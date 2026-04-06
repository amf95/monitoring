
>**Version control:**
>
> **OS**: ubuntu 24.04
>
> **alert-manager**: [alertmanager-0.31.1.linux-amd64.tar.gz](https://github.com/prometheus/alertmanager/releases/download/v0.31.1/alertmanager-0.31.1.linux-amd64.tar.gz)

---
## Open Config File:

```bash
vim /etc/alertmanager/alertmanager.yml
```

---

[Examples Source](https://samber.github.io/awesome-prometheus-alerts/rules.html#host-and-hardware)
## Check alertmanager.yml syntax:
```bash
amtool check-config /etc/alertmanager/alertmanager.yml
```

---
## Check .tmpl syntax:

>If file is ok it will render and expand templates.

```bash
amtool template render --template.glob='*.tmpl' --template.text='{{ template "custom_mail_html" . }}'
```

---
## Send Manual Alert:

```bash
curl -H 'Content-Type: application/json' -d '[{"labels":{"alertname":"test-alert"}}]' http://localhost:9093/api/v2/alerts
```

or 

```bash
amtool --alertmanager.url=http://localhost:9093/ alert add alertname="test" severity="just-a-test" job="test-job" instance="localhost" exporter="test-exporter" cluster="test-cluster"
```

or

```bash
amtool --alertmanager.url=http://localhost:9093/ alert add alertname="test" time="$(date +'%Y-%m-%d %H:%M:%S')"
```

---
## Alert Manager YAML:

```bash
vim /etc/alertmanager/alertmanager.yml
```

```yaml
route:
  group_by: ['alertname']
  
  # When a new group of alerts is created by an incoming alert, wait at
  # least 'group_wait' to send the initial notification.
  # This way ensures that you get multiple alerts for the same group that start
  # firing shortly after another are batched together on the first
  # nsend_resolved: trueotification.
  group_wait: 1s # time to wait after condition is met and alert triggered.
  
  # When the first notification was sent, wait 'group_interval' to send a batch
  # of new alerts that started firing for that group.
  group_interval: 5m

  # If an alert has successfully been sent, wait 'repeat_interval' to
  # resend them.
  repeat_interval: 1s

  # A default receiver.
  receiver: 'email-receivers'

receivers:
  - name: 'email-receivers'
    email_configs:
    - to: 'receiver.1@example.com,receiver.2@example.com'
      from: 'sender@@example.com'
      smarthost: 'smtp.gmail.com:587' # or any other
      auth_username: 'sender@@example.com'
      auth_identity: 'sender@@example.com'
      auth_password: '<sender@@example.com password here>'
      send_resolved: true

#---------------------------------------------

inhibit_rules:
  - source_match:
      severity: 'critical'
    target_match:
      severity: 'warning'
    equal: ['alertname', 'dev', 'instance']
```

---
## In Prometheus Config YAML:

>Add or Edit the following sections:

```yaml
# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
           - "localhost:9093"

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
   - "alert_rules.yml" # in the same directory of prometheus.yml

```

---
## Prometheus Rules YAML:

> `alert_rules.yml`: In prometheus configs.

```yaml
groups:
- name: alert.rules
  rules:
  - alert: InstanceDown
    expr: up == 0
    for: 1m
    labels:
      severity: "critical"
    annotations:
      summary: "Endpoint {{ $labels.instance }} down"
      description: "{{ $labels.instance }} of job {{ $labels.job }} has been down for more than 1 minutes."
  
  - alert: HostOutOfMemory
    expr: node_memory_MemAvailable / node_memory_MemTotal * 100 < 80
    for: 1m
    labels:
      severity: warning
    annotations:
      summary: "Host out of memory (instance {{ $labels.instance }})"
      description: "Node memory is filling up (< 25% left)\n  VALUE = {{ $value }}\n  LABELS: {{ $labels }}"


  - alert: HostOutOfDiskSpace
    expr: (node_filesystem_avail{mountpoint="/"}  * 100) / node_filesystem_size{mountpoint="/"} < 50
    for: 1s
    labels:
      severity: warning
    annotations:
      summary: "Host out of disk space (instance {{ $labels.instance }})"
      description: "Disk is almost full (< 50% left)\n  VALUE = {{ $value }}\n  LABELS: {{ $labels }}"


  - alert: HostHighCpuLoad
    expr: (sum by (instance) (irate(node_cpu{job="node_exporter_metrics",mode="idle"}[5m]))) > 80
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: "Host high CPU load (instance {{ $labels.instance }})"
      description: "CPU load is > 80%\n  VALUE = {{ $value }}\n  LABELS: {{ $labels }}"

```

## Examples:
### 1. **Basic Alerting Rule Example**

This example triggers an alert when the CPU usage of a node exceeds 80%.

```yaml
groups:
- name: node_alerts
  rules:
  - alert: HighCpuUsage
    expr: 100 - (avg by(instance) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 80
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "High CPU usage on {{ $labels.instance }}"
      description: "CPU usage on {{ $labels.instance }} is above 80% for the last 5 minutes."
```

- **`alert`**: The name of the alert.
- **`expr`**: The PromQL expression that defines the condition for the alert.
- **`for`**: The duration for which the condition must be true before the alert is triggered.
- **`labels`**: Additional labels attached to the alert (e.g., severity).
- **`annotations`**: Descriptive information about the alert.

---

### 2. **Memory Usage Alert**

This example triggers an alert when the memory usage of a node exceeds 90%.

```yaml
groups:
- name: node_alerts
  rules:
  - alert: HighMemoryUsage
    expr: (node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / node_memory_MemTotal_bytes * 100 > 90
    for: 10m
    labels:
      severity: warning
    annotations:
      summary: "High memory usage on {{ $labels.instance }}"
      description: "Memory usage on {{ $labels.instance }} is above 90% for the last 10 minutes."
```

---

### 3. **Disk Space Alert**

This example triggers an alert when the available disk space on a node is less than 10%.

```yaml
groups:
- name: node_alerts
  rules:
  - alert: LowDiskSpace
    expr: (node_filesystem_avail_bytes{mountpoint="/"} / node_filesystem_size_bytes{mountpoint="/"} * 100) < 10
    for: 15m
    labels:
      severity: critical
    annotations:
      summary: "Low disk space on {{ $labels.instance }}"
      description: "Disk space on {{ $labels.instance }} is less than 10% for the last 15 minutes."
```

---

### 4. **Service Down Alert**

This example triggers an alert when a service is down (e.g., HTTP server not responding).

```yaml
groups:
- name: service_alerts
  rules:
  - alert: ServiceDown
    expr: up{job="my_service"} == 0
    for: 2m
    labels:
      severity: critical
    annotations:
      summary: "Service {{ $labels.job }} is down on {{ $labels.instance }}"
      description: "Service {{ $labels.job }} on {{ $labels.instance }} has been down for the last 2 minutes."
```

---

### 5. **High Request Latency Alert**

This example triggers an alert when the 99th percentile request latency exceeds 1 second.

```yaml
groups:
- name: http_alerts
  rules:
  - alert: HighRequestLatency
    expr: histogram_quantile(0.99, sum(rate(http_request_duration_seconds_bucket{job="my_service"}[5m])) by (le)) > 1
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: "High request latency on {{ $labels.job }}"
      description: "99th percentile request latency on {{ $labels.job }} is above 1 second for the last 5 minutes."
```

---

### 6. **High Error Rate Alert**

This example triggers an alert when the error rate for HTTP requests exceeds 5%.

```yaml
groups:
- name: http_alerts
  rules:
  - alert: HighErrorRate
    expr: sum(rate(http_requests_total{job="my_service", status=~"5.."}[5m])) / sum(rate(http_requests_total{job="my_service"}[5m])) * 100 > 5
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "High error rate on {{ $labels.job }}"
      description: "Error rate on {{ $labels.job }} is above 5% for the last 5 minutes."
```

---

### 7. **Custom Alert with Multiple Conditions**

This example triggers an alert when both CPU and memory usage are high.

```yaml
groups:
- name: node_alerts
  rules:
  - alert: HighCpuAndMemoryUsage
    expr: |
      100 - (avg by(instance) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 80
      and
      (node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / node_memory_MemTotal_bytes * 100 > 90
    for: 10m
    labels:
      severity: critical
    annotations:
      summary: "High CPU and memory usage on {{ $labels.instance }}"
      description: "CPU usage is above 80% and memory usage is above 90% on {{ $labels.instance }} for the last 10 minutes."
```

---

### 8. **Alert for Missing Metrics**

This example triggers an alert if a specific metric is missing (e.g., a scrape failure).

```yaml
groups:
- name: scrape_alerts
  rules:
  - alert: MissingMetrics
    expr: absent(up{job="my_service"})
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "Metrics missing for {{ $labels.job }}"
      description: "No metrics have been scraped for {{ $labels.job }} in the last 5 minutes."
```

---

### 9. **Alert for High Network Traffic**
```yaml
​￼global:
  resolve_timeout: 5m

​￼route:
  receiver: 'multi-receiver'

​￼receivers:
  ​￼- name: 'multi-receiver'
    ​￼webhook_configs:
      ​￼- url: 'https://outlook.office.com/webhook/YOUR_WEBHOOK_URL'  # Microsoft Teams
        send_resolved: true
      ​￼- url: 'http://<telegram-bridge-service>/alert'  # Telegram
        send_resolved: true
```
This example triggers an alert when network traffic exceeds 1 Gbps.

```yaml
groups:
- name: network_alerts
  rules:
  - alert: HighNetworkTraffic
    expr: rate(node_network_receive_bytes_total{device="eth0"}[5m]) * 8 > 1e9
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: "High network traffic on {{ $labels.instance }}"
      description: "Network traffic on {{ $labels.instance }} exceeds 1 Gbps for the last 5 minutes."
```

---

### 10. **Alert for High Pod Restarts in Kubernetes**

This example triggers an alert when a Kubernetes pod restarts more than 5 times in 10 minutes.

```yaml
groups:
- name: kubernetes_alerts
  rules:
  - alert: HighPodRestarts
    expr: increase(kube_pod_container_status_restarts_total[10m]) > 5
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: "High pod restarts on {{ $labels.pod }}"
      description: "Pod {{ $labels.pod }} has restarted more than 5 times in the last 10 minutes."
```

---

### Applying the Rules

1. Save the rules in a file (e.g., `rules.yml`).
2. Add the `rule_files` directive to your `prometheus.yml` configuration file:

   ```yaml
   rule_files:
     - "rules.yml"
   ```

3. Reload Prometheus to apply the new rules:

   ```bash
   curl -X POST http://localhost:9090/-/reload
   ```

---

These examples demonstrate how to create alerting rules for common scenarios. You can customize the rules based on your specific monitoring needs and metrics.<<<<<<<<<<<<<<<<<<<<<<<