
>Port: 9090

>**TLS** configs can be found in the systemd section.

> Prometheus is a service that collects data from its exporters.

> The data collected is stored in a custom database(Chunks).

> Data can be queried with `PromQL` language.

> Queries can be run either directly in the prometheus web ui or throw the API.

>**Exporters EXP**: `blackbox exporter`: for end point metrics, `node exporter`: for OS and machine metrics, `nginx exporter`: extracts nginx service metrics, `mySQL exporter`: for mySQL database metrics, `process exporter`: for processes metrics, ... [Click for more about exporters](https://prometheus.io/docs/instrumenting/exporters/).

---

# How everything  works together?

![](assets/prometheus-system-architecture-and-how-it-works.svg)
