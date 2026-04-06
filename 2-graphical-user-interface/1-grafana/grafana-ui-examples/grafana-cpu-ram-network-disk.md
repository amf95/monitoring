```json
{
  "__inputs": [
    {
      "name": "DS_PROMETHEUS",
      "label": "prometheus",
      "description": "",
      "type": "datasource",
      "pluginId": "prometheus",
      "pluginName": "Prometheus"
    }
  ],
  "__elements": {},
  "__requires": [
    {
      "type": "grafana",
      "id": "grafana",
      "name": "Grafana",
      "version": "11.5.2"
    },
    {
      "type": "datasource",
      "id": "prometheus",
      "name": "Prometheus",
      "version": "1.0.0"
    },
    {
      "type": "panel",
      "id": "timeseries",
      "name": "Time series",
      "version": ""
    }
  ],
  "annotations": {
    "list": [
      {
        "builtIn": 1,
        "datasource": {
          "type": "grafana",
          "uid": "-- Grafana --"
        },
        "enable": true,
        "hide": true,
        "iconColor": "rgba(0, 211, 255, 1)",
        "name": "Annotations & Alerts",
        "type": "dashboard"
      }
    ]
  },
  "description": "custom dashboard to display all the metrics needed for a specific machine of a specific application.\nNOTE: Please choose <app>_job and select your desired machine to see its metrics.\nAuthor: Ahmed Fawzy",
  "editable": true,
  "fiscalYearStartMonth": 0,
  "graphTooltip": 0,
  "id": null,
  "links": [],
  "panels": [
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "description": "CPU time spent busy vs idle, split by activity type",
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "palette-classic"
          },
          "custom": {
            "axisBorderShow": false,
            "axisCenteredZero": false,
            "axisColorMode": "text",
            "axisLabel": "CPU Utilization",
            "axisPlacement": "auto",
            "barAlignment": 0,
            "barWidthFactor": 0.6,
            "drawStyle": "line",
            "fillOpacity": 30,
            "gradientMode": "none",
            "hideFrom": {
              "legend": false,
              "tooltip": false,
              "viz": false
            },
            "insertNulls": false,
            "lineInterpolation": "smooth",
            "lineStyle": {
              "fill": "solid"
            },
            "lineWidth": 2,
            "pointSize": 5,
            "scaleDistribution": {
              "type": "linear"
            },
            "showPoints": "never",
            "spanNulls": false,
            "stacking": {
              "group": "A",
              "mode": "percent"
            },
            "thresholdsStyle": {
              "mode": "off"
            }
          },
          "links": [],
          "mappings": [],
          "min": 0,
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "transparent",
                "value": null
              }
            ]
          },
          "unit": "percent"
        },
        "overrides": [
          {
            "matcher": {
              "id": "byName",
              "options": "utilzation"
            },
            "properties": [
              {
                "id": "color",
                "value": {
                  "fixedColor": "red",
                  "mode": "fixed"
                }
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Idle"
            },
            "properties": [
              {
                "id": "color",
                "value": {
                  "fixedColor": "blue",
                  "mode": "fixed"
                }
              }
            ]
          }
        ]
      },
      "gridPos": {
        "h": 24,
        "w": 24,
        "x": 0,
        "y": 0
      },
      "id": 9,
      "options": {
        "legend": {
          "calcs": [
            "lastNotNull"
          ],
          "displayMode": "list",
          "placement": "bottom",
          "showLegend": true,
          "width": 250
        },
        "tooltip": {
          "hideZeros": false,
          "mode": "multi",
          "sort": "none"
        }
      },
      "pluginVersion": "11.5.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "# cpu utilzation:\n100 * (1 - avg(rate(node_cpu_seconds_total{mode=\"idle\", job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}[1m])))",
          "hide": false,
          "instant": false,
          "interval": "",
          "legendFormat": "utilzation",
          "range": true,
          "refId": "B"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "# cpu idle\n# should be a the bottom of the queries.\nsum(irate(node_cpu_seconds_total{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\", mode=\"idle\"}[$__rate_interval])) / scalar(count(count(node_cpu_seconds_total{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}) by (cpu))) * 100",
          "format": "time_series",
          "hide": false,
          "intervalFactor": 1,
          "legendFormat": "Idle",
          "range": true,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "CPU Load",
      "type": "timeseries"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "description": "Per-interface network traffic (receive and transmit) in bits per second",
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "palette-classic"
          },
          "custom": {
            "axisBorderShow": false,
            "axisCenteredZero": false,
            "axisColorMode": "text",
            "axisLabel": "Rx (–) / Tx (+)",
            "axisPlacement": "auto",
            "barAlignment": 0,
            "barWidthFactor": 0.6,
            "drawStyle": "line",
            "fillOpacity": 40,
            "gradientMode": "none",
            "hideFrom": {
              "legend": false,
              "tooltip": false,
              "viz": false
            },
            "insertNulls": false,
            "lineInterpolation": "linear",
            "lineWidth": 1,
            "pointSize": 5,
            "scaleDistribution": {
              "type": "linear"
            },
            "showPoints": "never",
            "spanNulls": false,
            "stacking": {
              "group": "A",
              "mode": "none"
            },
            "thresholdsStyle": {
              "mode": "off"
            }
          },
          "links": [],
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": null
              }
            ]
          },
          "unit": "bps"
        },
        "overrides": [
          {
            "matcher": {
              "id": "byRegexp",
              "options": "/.*Rx.*/"
            },
            "properties": [
              {
                "id": "custom.transform",
                "value": "negative-Y"
              }
            ]
          }
        ]
      },
      "gridPos": {
        "h": 24,
        "w": 24,
        "x": 0,
        "y": 24
      },
      "id": 20,
      "options": {
        "legend": {
          "calcs": [
            "lastNotNull"
          ],
          "displayMode": "list",
          "placement": "bottom",
          "showLegend": true
        },
        "tooltip": {
          "hideZeros": false,
          "mode": "multi",
          "sort": "none"
        }
      },
      "pluginVersion": "11.5.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "rate(node_network_receive_bytes_total{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}[$__rate_interval])*8",
          "format": "time_series",
          "intervalFactor": 1,
          "legendFormat": "Rx {{device}} ",
          "range": true,
          "refId": "A",
          "step": 240
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "rate(node_network_transmit_bytes_total{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}[$__rate_interval])*8",
          "format": "time_series",
          "intervalFactor": 1,
          "legendFormat": "Tx {{device}} ",
          "range": true,
          "refId": "B",
          "step": 240
        }
      ],
      "title": "Network Traffic Basic",
      "type": "timeseries"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "description": "Number of bytes read from or written to the device per second",
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "palette-classic"
          },
          "custom": {
            "axisBorderShow": false,
            "axisCenteredZero": false,
            "axisColorMode": "text",
            "axisLabel": "read (–) / write (+)",
            "axisPlacement": "auto",
            "barAlignment": 0,
            "barWidthFactor": 0.6,
            "drawStyle": "line",
            "fillOpacity": 20,
            "gradientMode": "none",
            "hideFrom": {
              "legend": false,
              "tooltip": false,
              "viz": false
            },
            "insertNulls": false,
            "lineInterpolation": "linear",
            "lineWidth": 1,
            "pointSize": 5,
            "scaleDistribution": {
              "type": "linear"
            },
            "showPoints": "never",
            "spanNulls": false,
            "stacking": {
              "group": "A",
              "mode": "none"
            },
            "thresholdsStyle": {
              "mode": "off"
            }
          },
          "links": [],
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": null
              }
            ]
          },
          "unit": "Bps"
        },
        "overrides": [
          {
            "matcher": {
              "id": "byRegexp",
              "options": "/.*Read.*/"
            },
            "properties": [
              {
                "id": "custom.transform",
                "value": "negative-Y"
              }
            ]
          },
          {
            "matcher": {
              "id": "byRegexp",
              "options": "/sda.*/"
            },
            "properties": [
              {
                "id": "color",
                "value": {
                  "fixedColor": "orange",
                  "mode": "fixed"
                }
              }
            ]
          }
        ]
      },
      "gridPos": {
        "h": 9,
        "w": 24,
        "x": 0,
        "y": 48
      },
      "id": 27,
      "options": {
        "legend": {
          "calcs": [
            "min",
            "mean",
            "max",
            "lastNotNull"
          ],
          "displayMode": "table",
          "placement": "bottom",
          "showLegend": true
        },
        "tooltip": {
          "hideZeros": false,
          "mode": "single",
          "sort": "none"
        }
      },
      "pluginVersion": "11.5.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "irate(node_disk_read_bytes_total{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}[1m])",
          "format": "time_series",
          "intervalFactor": 1,
          "legendFormat": "{{device}} - Read",
          "range": true,
          "refId": "A",
          "step": 240
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "irate(node_disk_written_bytes_total{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}[1m])",
          "format": "time_series",
          "instant": false,
          "intervalFactor": 1,
          "legendFormat": "{{device}} - Write",
          "range": true,
          "refId": "B",
          "step": 240
        }
      ],
      "title": "Disk Read/Write Data",
      "type": "timeseries"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "palette-classic"
          },
          "custom": {
            "axisBorderShow": true,
            "axisCenteredZero": false,
            "axisColorMode": "text",
            "axisLabel": "RAM Usage",
            "axisPlacement": "auto",
            "barAlignment": 0,
            "barWidthFactor": 0.6,
            "drawStyle": "line",
            "fillOpacity": 7,
            "gradientMode": "none",
            "hideFrom": {
              "legend": false,
              "tooltip": false,
              "viz": false
            },
            "insertNulls": false,
            "lineInterpolation": "linear",
            "lineWidth": 2,
            "pointSize": 5,
            "scaleDistribution": {
              "type": "linear"
            },
            "showPoints": "auto",
            "spanNulls": false,
            "stacking": {
              "group": "A",
              "mode": "none"
            },
            "thresholdsStyle": {
              "mode": "off"
            }
          },
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "transparent",
                "value": null
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": [
          {
            "matcher": {
              "id": "byName",
              "options": "Total"
            },
            "properties": [
              {
                "id": "color",
                "value": {
                  "fixedColor": "purple",
                  "mode": "fixed"
                }
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Used"
            },
            "properties": [
              {
                "id": "color",
                "value": {
                  "fixedColor": "red",
                  "mode": "fixed"
                }
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Free"
            },
            "properties": [
              {
                "id": "color",
                "value": {
                  "fixedColor": "green",
                  "mode": "fixed"
                }
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Cache+Buffer"
            },
            "properties": [
              {
                "id": "color",
                "value": {
                  "fixedColor": "blue",
                  "mode": "fixed"
                }
              }
            ]
          }
        ]
      },
      "gridPos": {
        "h": 22,
        "w": 24,
        "x": 0,
        "y": 57
      },
      "id": 5,
      "options": {
        "legend": {
          "calcs": [
            "lastNotNull"
          ],
          "displayMode": "list",
          "placement": "bottom",
          "showLegend": true
        },
        "tooltip": {
          "hideZeros": false,
          "mode": "single",
          "sort": "none"
        }
      },
      "pluginVersion": "11.5.2",
      "targets": [
        {
          "editorMode": "code",
          "expr": "# total\nnode_memory_MemTotal_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}",
          "legendFormat": "Total",
          "range": true,
          "refId": "A",
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          }
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "# used\nnode_memory_MemTotal_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n - node_memory_MemFree_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n - (node_memory_Cached_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n + node_memory_Buffers_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n + node_memory_SReclaimable_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"})",
          "hide": false,
          "instant": false,
          "legendFormat": "Used",
          "range": true,
          "refId": "B"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "# free\nnode_memory_MemFree_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}",
          "hide": false,
          "instant": false,
          "legendFormat": "Free",
          "range": true,
          "refId": "C"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "# chache + buffer\nnode_memory_Cached_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"} \n+ node_memory_Buffers_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"} \n+ node_memory_SReclaimable_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}",
          "hide": false,
          "instant": false,
          "legendFormat": "Cache+Buffer",
          "range": true,
          "refId": "D"
        }
      ],
      "title": "Memory(RAM)",
      "type": "timeseries"
    }
  ],
  "refresh": "5s",
  "schemaVersion": 40,
  "tags": [],
  "templating": {
    "list": [
      {
        "current": {},
        "definition": "label_values(process_cpu_seconds_total,job)",
        "label": "Node Exporter Job",
        "name": "node_exporter_job",
        "options": [],
        "query": {
          "qryType": 1,
          "query": "label_values(process_cpu_seconds_total,job)",
          "refId": "PrometheusVariableQueryEditor-VariableQuery"
        },
        "refresh": 1,
        "regex": "",
        "sort": 1,
        "type": "query"
      },
      {
        "current": {},
        "definition": "label_values(node_cpu_seconds_total{job=\"$node_exporter_job\"},instance)",
        "label": "Node Exporter Instance",
        "name": "node_exporter_instance",
        "options": [],
        "query": {
          "qryType": 1,
          "query": "label_values(node_cpu_seconds_total{job=\"$node_exporter_job\"},instance)",
          "refId": "PrometheusVariableQueryEditor-VariableQuery"
        },
        "refresh": 1,
        "regex": "",
        "sort": 1,
        "type": "query"
      }
    ]
  },
  "time": {
    "from": "now-30m",
    "to": "now"
  },
  "timepicker": {
    "refresh_intervals": [
      "5s",
      "10s",
      "30s",
      "1m"
    ]
  },
  "timezone": "Africa/Cairo",
  "title": "Example: CPU, Memory, Network, Disk Metrics Charts AMF95's Edition.",
  "uid": "dfewa560p7xtsf",
  "version": 6,
  "weekStart": ""
}
```