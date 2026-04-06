
>Port: 9093

![](assets/alert-manager-system-architecture-and-how-it-works.svg)

Prometheus Alertmanager is a component of the Prometheus monitoring system that handles alerts sent by the Prometheus server. It is responsible for managing, deduplicating, grouping, and routing alerts to the appropriate receivers, such as email, PagerDuty, Slack, or other notification channels. The Alertmanager also handles silencing and inhibition of alerts, ensuring that only relevant and actionable alerts are sent to the right people.

Here are the key features and functionalities of Prometheus Alertmanager:

1. **Alert Deduplication**: When multiple alerts are generated for the same issue, Alertmanager deduplicates them to avoid overwhelming the receivers with redundant notifications.

2. **Grouping**: Alerts that are related can be grouped together into a single notification. This is useful when a single issue triggers multiple alerts, and you want to receive a consolidated notification rather than individual ones.

3. **Routing**: Alertmanager allows you to configure routing rules to send alerts to different receivers based on labels. For example, you can route critical alerts to a paging system and less critical ones to an email group.

4. **Silencing**: You can temporarily silence alerts based on matching criteria. This is useful during maintenance windows or when you know an issue is being worked on and do not want to be notified.

5. **Inhibition**: Inhibition rules allow you to suppress certain alerts if other alerts are already firing. For example, if a cluster-wide outage is detected, you might want to inhibit all alerts related to individual nodes within that cluster.

6. **Notification Channels**: Alertmanager supports various notification channels, including email, Slack, PagerDuty, OpsGenie, and more. You can configure multiple receivers and route alerts to them based on severity or other criteria.

7. **High Availability**: Alertmanager can be run in a high-availability mode where multiple instances run in parallel. They communicate with each other to ensure that alerts are not duplicated and that the system remains resilient to failures.

### How It Works

1. **Alert Generation**: Prometheus server evaluates alerting rules based on the metrics it collects. When a rule condition is met, it generates an alert and sends it to the Alertmanager.

2. **Alert Processing**: Alertmanager receives the alerts and processes them according to the configured rules. This includes deduplication, grouping, and routing.

3. **Notification**: Once the alerts are processed, Alertmanager sends notifications to the configured receivers.

### Example Configuration

Here is a simple example of an Alertmanager configuration file (`alertmanager.yml`):

```yaml
global:
  resolve_timeout: 5m

route:
  receiver: 'email-notifications'
  group_by: ['alertname', 'cluster', 'service']
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 3h
  routes:
  - match:
      severity: 'critical'
    receiver: 'pagerduty'
  - match:
      severity: 'warning'
    receiver: 'slack-notifications'

receivers:
- name: 'email-notifications'
  email_configs:
  - to: 'team@example.com'

- name: 'pagerduty'
  pagerduty_configs:
  - service_key: 'your-pagerduty-key'

- name: 'slack-notifications'
  slack_configs:
  - api_url: 'https://hooks.slack.com/services/your/slack/webhook'
    channel: '#alerts'
```

In this configuration:

- Alerts are grouped by `alertname`, `cluster`, and `service`.
- Alerts with severity `critical` are routed to PagerDuty.
- Alerts with severity `warning` are routed to a Slack channel.
- All other alerts are sent to an email address.

### Conclusion

Prometheus Alertmanager is a powerful tool for managing alerts in a Prometheus-based monitoring system. It helps ensure that alerts are meaningful, actionable, and sent to the right people through the appropriate channels. By configuring deduplication, grouping, routing, silencing, and inhibition, you can tailor the alerting system to meet the specific needs of your organization.