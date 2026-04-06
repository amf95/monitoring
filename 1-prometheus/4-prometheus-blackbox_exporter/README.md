
---

>Port: 9115

>**TLS** configs can be found in the systemd section.

[video source](https://www.youtube.com/watch?v=0Ec_VeO60m8&t=1806s)

[web source](https://www.infracloud.io/blogs/monitoring-endpoints-kubernetes-blackbox-exporter/)
[web source](https://www.opsramp.com/guides/prometheus-monitoring/prometheus-blackbox-exporter/)

![](assets/prometheus-blackbox-exporter-system-architecture-and-how-it-works.svg)

### **What is Prometheus Blackbox Exporter?**
The Blackbox Exporter is a standalone application that performs **active probing** of **endpoints**. It sends requests to targets (e.g., websites, APIs, DNS servers, or TCP services) and collects metrics about the results of those probes. These metrics are then exposed in a format that Prometheus can scrape and store for further analysis, visualization, and alerting.

---

### **Key Features**
1. **Multi-Protocol Support**:
   - Blackbox can probe endpoints using multiple protocols:
     - **HTTP/HTTPS**: Checks if a web service is reachable, measures response times, and validates status codes or response content.
     - **DNS**: Tests DNS resolution and validates specific record types (e.g., A, AAAA, MX).
     - **TCP**: Verifies if a TCP port is open and responsive.
     - **ICMP**: Pings hosts to check network connectivity.
     - **gRPC**: Probes gRPC services for availability.

2. **Customizable Probes**:
   - You can define **modules** in the Blackbox configuration to specify how probes should behave. For example:
     - Validate HTTP response codes (e.g., only consider a probe successful if the response is 200 OK).
     - Check for specific content in the response body.
     - Verify TLS certificate expiration dates.
     - Set timeouts and retries for probes.

3. **Metrics Export**:
   - Blackbox exposes metrics in a format that Prometheus can scrape. These metrics include:
     - `probe_success`: Whether the probe was successful (1 for success, 0 for failure).
     - `probe_duration_seconds`: How long the probe took to complete.
     - `probe_http_status_code`: The HTTP status code returned by the target.
     - `probe_ssl_earliest_cert_expiry`: The earliest expiration date of the TLS certificate.

4. **Integration with Prometheus**:
   - Blackbox works seamlessly with Prometheus. Prometheus scrapes the metrics exposed by Blackbox and stores them for querying, visualization, and alerting.

5. **Flexible Configuration**:
   - Blackbox is configured using a YAML file (`blackbox.yml`), where you can define multiple modules for different types of probes. Each module can have specific settings, such as timeouts, headers, or DNS query types.

6. **Scalability**:
   - Blackbox is lightweight and can be deployed across multiple instances to monitor large-scale environments.

---

### **How It Works**
1. **Probing**:
   - Blackbox sends requests to the target endpoints based on the configured modules.
   - For example, if you configure an HTTP probe, Blackbox will send an HTTP request to the target and measure the response time, status code, and other details.

2. **Metrics Export**:
   - After probing, Blackbox exposes metrics in a format that Prometheus can scrape. These metrics are available at the `/metrics` endpoint of the Blackbox Exporter.

3. **Prometheus Scraping**:
   - Prometheus periodically scrapes the metrics exposed by Blackbox and stores them in its time-series database.

4. **Visualization and Alerting**:
   - The collected metrics can be visualized using tools like **Grafana**.
   - Prometheus **Alertmanager** can be configured to trigger alerts based on probe results (e.g., if a website is down or if response times exceed a threshold).

---

### **Use Cases**
1. **Website Monitoring**:
   - Check if a website is up and measure response times.
   - Validate HTTP status codes and response content.

2. **API Monitoring**:
   - Monitor the availability and performance of RESTful APIs or gRPC services.

3. **DNS Monitoring**:
   - Verify DNS resolution and ensure DNS records are correct.

4. **Network Diagnostics**:
   - Test network connectivity using ICMP (ping) or TCP probes.

5. **Certificate Expiry Monitoring**:
   - Check the validity of TLS certificates and alert before they expire.

---
### **Metrics Exposed by Blackbox**
Here are some key metrics exposed by Blackbox:
- `probe_success`: Whether the probe was successful (1 for success, 0 for failure).
- `probe_duration_seconds`: Duration of the probe in seconds.
- `probe_http_status_code`: HTTP status code returned by the target.
- `probe_ssl_earliest_cert_expiry`: Earliest expiration date of the TLS certificate.
- `probe_icmp_duration_seconds`: Duration of ICMP (ping) probes.

---

### **Key Differences: TCP vs HTTP Probe**

| Feature                | **TCP Probe**                     | **HTTP Probe**                                 |
| ---------------------- | --------------------------------- | ---------------------------------------------- |
| **Layer**              | Transport Layer (TCP)             | Application Layer (HTTP)                       |
| **Purpose**            | Check if a TCP port is open       | Check if a full HTTP service is responding     |
| **Used For**           | Is something listening on a port? | Is a web service behaving correctly?           |
| **TLS Supported?**     | Yes                               | Yes (via HTTPS)                                |
| **Probes via Port**    | Yes (e.g., port 22, 80, 443)      | Yes (typically 80, 443)                        |
| **Returns what?**      | TCP connect time                  | HTTP status code, response time, body match    |
| **Can check content?** | ❌ No                              | ✅ Yes (regex match on body, status code, etc.) |
| **Examples**           | Ping SSH (port 22), NFS (2049)    | Check website, REST API, etc.                  |
