
To modify an **Alertmanager email template** in Prometheus, you will need to update the **receiver configuration** section for email notifications. Here’s how you can customize the email notification template using Alertmanager.

### Example of Modified Alertmanager Email Template:

```yaml
global:
  resolve_timeout: 5m
  smtp_smarthost: 'smtp.example.com:587'  # SMTP server
  smtp_from: 'alertmanager@example.com'   # Sender email address
  smtp_auth_username: 'smtp_user'         # SMTP username
  smtp_auth_password: 'smtp_password'     # SMTP password

route:
  group_by: ['alertname', 'severity']
  receiver: 'email-notifications'
  routes:
    - match:
        severity: 'critical'
      receiver: 'email-critical'
    - match:
        severity: 'warning'
      receiver: 'email-warning'

receivers:
  - name: 'email-notifications'
    email_configs:
      - send_resolved: true
        to: 'team@example.com'  # Default email address for general alerts
        subject: "[AlertManager] Alert: {{ .CommonLabels.alertname }} - Severity: {{ .CommonLabels.severity }}"
        text: |
          *Alert:* {{ .CommonLabels.alertname }}
          *Severity:* {{ .CommonLabels.severity }}
          *Instance:* {{ .CommonLabels.instance }}
          *Description:* {{ .CommonAnnotations.description }}
          *Runbook:* [View Runbook](https://your-runbook-url.com)
          *Details:*
          - Summary: {{ .CommonAnnotations.summary }}
          - [Alert Details]({{ .CommonLabels.alertname | printf "https://your-alert-management-system.com/%s" .CommonLabels.alertname }})
          - [Prometheus Graph](https://prometheus.example.com/graph?g0.expr={{ .CommonLabels.alertname }}&g0.tab=0)

  - name: 'email-critical'
    email_configs:
      - send_resolved: true
        to: 'critical-team@example.com'  # Email for critical alerts
        subject: "[Critical] Alert: {{ .CommonLabels.alertname }} - Severity: {{ .CommonLabels.severity }}"
        text: |
          *Critical Alert:* {{ .CommonLabels.alertname }}
          *Severity:* {{ .CommonLabels.severity }}
          *Instance:* {{ .CommonLabels.instance }}
          *Description:* {{ .CommonAnnotations.description }}
          *Runbook:* [View Runbook](https://your-runbook-url.com)
          *Details:*
          - Summary: {{ .CommonAnnotations.summary }}
          - [Alert Details]({{ .CommonLabels.alertname | printf "https://your-alert-management-system.com/%s" .CommonLabels.alertname }})
          - [Prometheus Graph](https://prometheus.example.com/graph?g0.expr={{ .CommonLabels.alertname }}&g0.tab=0)

  - name: 'email-warning'
    email_configs:
      - send_resolved: true
        to: 'warning-team@example.com'  # Email for warning alerts
        subject: "[Warning] Alert: {{ .CommonLabels.alertname }} - Severity: {{ .CommonLabels.severity }}"
        text: |
          *Warning Alert:* {{ .CommonLabels.alertname }}
          *Severity:* {{ .CommonLabels.severity }}
          *Instance:* {{ .CommonLabels.instance }}
          *Description:* {{ .CommonAnnotations.description }}
          *Runbook:* [View Runbook](https://your-runbook-url.com)
          *Details:*
          - Summary: {{ .CommonAnnotations.summary }}
          - [Alert Details]({{ .CommonLabels.alertname | printf "https://your-alert-management-system.com/%s" .CommonLabels.alertname }})
          - [Prometheus Graph](https://prometheus.example.com/graph?g0.expr={{ .CommonLabels.alertname }}&g0.tab=0)
```

### Explanation of Key Elements:

- **Global Configuration**:
    
    - `smtp_smarthost`: The SMTP server to send emails.
    - `smtp_from`: The "From" email address used for sending the alerts.
    - `smtp_auth_username` & `smtp_auth_password`: Authentication credentials for the SMTP server.
- **Route Configuration**:
    
    - Alerts are routed to different receivers based on the `severity` label. You can customize the severity levels (e.g., critical, warning) to route the emails to different teams or email addresses.
- **Receiver Configurations**:
    
    - **email-notifications**: Default email receiver for general alerts.
    - **email-critical**: Specific receiver for critical alerts.
    - **email-warning**: Specific receiver for warning alerts.
- **Template for Email Content**:
    
    - **Subject**: Customizes the email subject with the alert name and severity (e.g., "[AlertManager] Alert: High CPU Usage - Severity: critical").
    - **Body (Text)**: The email body is created using Go templating with placeholders:
        - `{{ .CommonLabels.alertname }}`: The alert's name (e.g., "HighCpuUsage").
        - `{{ .CommonLabels.severity }}`: The severity level (e.g., "critical", "warning").
        - `{{ .CommonLabels.instance }}`: The instance where the alert occurred.
        - `{{ .CommonAnnotations.description }}`: Description of the alert.
        - `{{ .CommonAnnotations.summary }}`: A short summary of the alert.
        - **Links**:
            - `Runbook`: Links to a runbook that provides instructions on how to resolve the issue.
            - `Alert Details`: A link to the alert details in your alert management system.
            - `Prometheus Graph`: A link to view the Prometheus graph related to the alert.

### Key Points:

1. **Customizable Subject/Body**:  
    You can tailor the subject and body of the email to match your organization's needs, using templating to make the notifications more informative and actionable.
    
2. **Routing Alerts**:  
    Alerts are routed based on severity. You can add more routes based on labels, such as instance, job, or custom labels.
    
3. **Runbook Integration**:  
    Link to an online runbook that provides instructions for resolving the issue directly within the email notification.
    
4. **Alert Management System Link**:  
    You can dynamically link to your alert management system to provide more detailed context for each alert.
    

### Useful Resources:

- [Prometheus Alertmanager Documentation](https://prometheus.io/docs/alerting/latest/alertmanager/)
- [Alertmanager Email Configuration](https://prometheus.io/docs/alerting/latest/configuration/#email-configurations)

Would you like to modify this template further or add specific features?