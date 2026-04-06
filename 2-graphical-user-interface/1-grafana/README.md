
>Port: 3000

>**TLS** configs can be found in the config section.

![](assets/grafana-loki-prometheus-system-architecture-and-how-it-works.svg)

> in the web-ui be careful with base 10 and base 2 units.
>
>reference: https://wiki.ubuntu.com/UnitsPolicy
>
>Disk, Network: si, base 10.
>
>Memory: iec, base 2.

**Grafana** is an open-source analytics and monitoring platform that allows you to visualize and query data from various sources, including Prometheus, Loki, InfluxDB, Elasticsearch, and others. It is widely used to create interactive dashboards for real-time monitoring and performance analysis. Grafana provides a **Graphical User Interface (GUI)** to interact with your data sources and visualize metrics, logs, and other data.

### Key Features of Grafana GUI

Here’s a detailed overview of the key components and features of the **Grafana GUI**:

---

### 1. **Login Screen**

When you access Grafana, it usually starts with a login screen where you can authenticate:

- **Default credentials** (unless customized):
  - **Username**: `admin`
  - **Password**: `admin`
  
  On first login, you’ll be prompted to change the default password.

---

### 2. **Main Dashboard Interface**

Once logged in, the main Grafana GUI consists of several components:

#### A. **Navigation Bar (Left Sidebar)**
The left sidebar provides access to the most common functions and features in Grafana.

- **Home**: Access the home dashboard (or the default dashboard view).
- **Dashboard**: Create new dashboards, manage existing ones, and access public dashboards. You can also import dashboards from the Grafana Dashboards repository.
- **Explore**: The "Explore" feature allows you to interactively query data sources and run ad-hoc queries to troubleshoot or view data in real-time.
- **Alerts**: Set up and manage alerting rules that notify users when certain thresholds are met (e.g., when a metric exceeds a value).
- **Data Sources**: Add or configure different data sources (Prometheus, InfluxDB, Elasticsearch, etc.).
- **Configuration**: Manage settings, users, teams, and other advanced configurations.

#### B. **Dashboards**
Dashboards are the central part of Grafana’s GUI.

- **Panels**: A dashboard consists of one or more panels. Panels can display data in various formats such as:
  - **Graph**: Display time-series data.
  - **Table**: Display data in tabular format.
  - **SingleStat**: Display a single value.
  - **Text**: Display text or markdown.
  - **Logs**: Display logs from sources like Loki.
  - **Gauge**: Display metrics like progress bars or speedometers.
  
- **Variables**: Grafana supports dynamic dashboards where you can create variables (e.g., to select different servers or time periods). Variables can be used in panel queries or panel titles.

#### C. **Query Editor**
The query editor allows you to create and customize queries for your data source.

- **Data Source**: Select the data source (e.g., Prometheus, InfluxDB, etc.).
- **Metrics**: Choose which metric or series you want to visualize.
- **Filter/Labels**: Filter the data to narrow down the results based on labels or tags (e.g., specific instances, regions, etc.).
- **Time Range**: Grafana allows you to select custom time ranges for your data, and you can set global time ranges for all panels on a dashboard.

#### D. **Time Picker**
Grafana provides a time picker in the top-right corner of the GUI that allows you to set the time range for the data you want to view:

- **Last 5 minutes, 1 hour, 24 hours**: Predefined time ranges.
- **Custom range**: Set a specific start and end time for viewing data.
- **Live**: Continuously update the data in real-time.

---

### 3. **Dashboard Features**

#### A. **Dashboard Settings**
You can customize and manage dashboards in Grafana using the settings menu for each dashboard. Some features include:

- **Renaming**: Change the name of the dashboard.
- **Star**: Star a dashboard to mark it as a favorite.
- **Permissions**: Control who has access to the dashboard and what they can do (view, edit, etc.).
- **JSON model**: Export the dashboard as a JSON model to share or back up configurations.
- **Sharing**: Share the dashboard with others using a link or embedding it in another web page.

#### B. **Panel Settings**
Panels can be configured with individual settings, including:

- **Panel Title**: Customize the panel title.
- **Data Format**: Choose between formats like time-series data, logs, and tables.
- **Legend & Tooltip**: Customize how the legend and tooltips appear for better clarity.
- **Thresholds & Alerts**: Set thresholds for visualizing values on graphs (e.g., color changes based on values) and create alerts.

#### C. **Visualization Types**
Grafana supports many visualization types, including:

- **Graph**: For time-series data (e.g., CPU usage over time).
- **Table**: For tabular data (e.g., logs or metrics with multiple attributes).
- **Singlestat**: Display a single, calculated value (e.g., average CPU usage).
- **Gauge**: Display metrics like system health or usage in a radial gauge format.
- **Bar Gauge**: A bar representing a single metric.
- **Pie Chart**: Display metric distributions as slices of a pie.
- **Logs**: View logs (especially useful when combined with Loki).
- **Heatmap**: Useful for visualizing density and distribution of data over time.

---

### 4. **Alerts and Notifications**

Grafana allows users to set up alerts based on conditions or thresholds, and these alerts can trigger notifications through various channels.

- **Alert Rules**: Define conditions for alerting (e.g., CPU usage over 80%).
- **Notification Channels**: Grafana supports multiple notification channels like email, Slack, Webhook, PagerDuty, and others.
- **Alert List**: A central place to view and manage active, pending, or resolved alerts.

---

### 5. **Explore Mode**

The **Explore** feature in Grafana allows you to run queries interactively and see the results in real-time, which is helpful for troubleshooting and debugging.

- **Query Builder**: Select data sources, choose metrics, and apply filters to analyze the data.
- **Live Tail Logs**: If integrated with **Loki** (Grafana's log aggregation tool), you can view and query logs in real-time.
- **History**: Grafana stores the queries you run in Explore mode, so you can revisit them later.

---

### 6. **Plugins and Extensions**

Grafana has a plugin architecture that allows users to extend its functionality. There are two main types of plugins:

- **Data Source Plugins**: Integrate Grafana with additional data sources (e.g., MySQL, PostgreSQL, Elasticsearch, etc.).
- **Panel Plugins**: Add new visualizations or panels for your dashboards (e.g., pie charts, heatmaps, etc.).
  
You can explore and install plugins directly from the **Configuration** section of the Grafana GUI.

---

### 7. **User Management**

Grafana allows you to manage users and set permissions:

- **Users**: Create and manage users, assign roles, and set permissions.
- **Teams**: Group users into teams for easier collaboration.
- **Role-based Access Control (RBAC)**: Control access levels (e.g., viewer, editor, admin) to dashboards and other Grafana features.

---

### 8. **Grafana Security**

- **Authentication**: Grafana supports multiple authentication mechanisms, including basic authentication, OAuth, LDAP, and others.
- **Permissions**: Fine-grained permissions can be set on dashboards and data sources, ensuring only authorized users can access or modify them.

---

### 9. **Customizing Grafana**

Grafana is highly customizable:

- **Themes**: Choose between light or dark themes, or create custom themes.
- **Custom Dashboards**: Build dashboards to meet specific needs (e.g., for monitoring infrastructure, applications, or network).
- **API**: Grafana provides a REST API to automate tasks like dashboard creation or alert management.

---

### Summary

Grafana’s GUI is powerful for building visualizations and dashboards from different data sources. Its features include:

- Real-time query and visualization capabilities.
- A variety of visualization types (graphs, tables, logs, gauges).
- Alerts and notifications for proactive monitoring.
- Extensive plugin support to extend data sources and visualizations.
- User management and role-based access control.
- Easy-to-use interface for exploring data and troubleshooting.

Grafana is widely used for monitoring applications, servers, databases, and cloud infrastructure, as well as for analyzing time-series data in real-time.

Let me know if you need more details on specific features or functionality!