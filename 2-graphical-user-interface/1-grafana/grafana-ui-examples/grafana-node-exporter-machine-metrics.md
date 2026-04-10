

![](../../../../monitoring/graphical-user-interface/grafana/assets/node-exporter-example.png)

---

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
      "type": "panel",
      "id": "gauge",
      "name": "Gauge",
      "version": ""
    },
    {
      "type": "grafana",
      "id": "grafana",
      "name": "Grafana",
      "version": "12.4.2"
    },
    {
      "type": "panel",
      "id": "piechart",
      "name": "Pie chart",
      "version": ""
    },
    {
      "type": "datasource",
      "id": "prometheus",
      "name": "Prometheus",
      "version": "1.0.0"
    },
    {
      "type": "panel",
      "id": "stat",
      "name": "Stat",
      "version": ""
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
  "links": [],
  "panels": [
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
            "hideFrom": {
              "legend": false,
              "tooltip": false,
              "viz": false
            }
          },
          "fieldMinMax": false,
          "mappings": [],
          "min": 0,
          "unit": "bytes"
        },
        "overrides": [
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
        "h": 13,
        "w": 4,
        "x": 0,
        "y": 0
      },
      "id": 2,
      "options": {
        "displayLabels": [
          "name",
          "value"
        ],
        "legend": {
          "displayMode": "table",
          "placement": "bottom",
          "showLegend": true,
          "values": [
            "percent",
            "value"
          ]
        },
        "pieType": "donut",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "sort": "desc",
        "tooltip": {
          "hideZeros": false,
          "mode": "single",
          "sort": "none"
        }
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "disableTextWrap": false,
          "editorMode": "code",
          "expr": "node_memory_MemFree_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}",
          "fullMetaSearch": false,
          "includeNullMetadata": true,
          "legendFormat": "Free",
          "range": true,
          "refId": "A",
          "useBackend": false
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "node_memory_MemTotal_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n - node_memory_MemFree_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n - (node_memory_Cached_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n + node_memory_Buffers_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n + node_memory_SReclaimable_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"})",
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
          "expr": "node_memory_Cached_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"} \n+ node_memory_Buffers_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"} \n+ node_memory_SReclaimable_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}",
          "instant": false,
          "legendFormat": "Cache+Buffer",
          "range": true,
          "refId": "C"
        }
      ],
      "title": "Memory(RAM):",
      "type": "piechart"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "purple",
                "value": 0
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 4,
        "w": 2,
        "x": 4,
        "y": 0
      },
      "id": 26,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "auto",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "node_memory_MemTotal_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "RAM Total:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "purple",
                "value": 0
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 4,
        "w": 2,
        "x": 6,
        "y": 0
      },
      "id": 25,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "auto",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "node_memory_SwapTotal_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "SWAP Total:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "red",
                "value": 0
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 2,
        "x": 8,
        "y": 0
      },
      "id": 33,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "value",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "node_memory_SwapTotal_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"} - node_memory_SwapFree_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "SWAP Used:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "displayName": "CPU Utilization",
          "mappings": [],
          "max": 100,
          "min": 0,
          "thresholds": {
            "mode": "percentage",
            "steps": [
              {
                "color": "green",
                "value": 0
              },
              {
                "color": "#EAB839",
                "value": 33
              },
              {
                "color": "red",
                "value": 66
              }
            ]
          },
          "unit": "percent"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 7,
        "w": 4,
        "x": 10,
        "y": 0
      },
      "id": 8,
      "options": {
        "minVizHeight": 75,
        "minVizWidth": 75,
        "orientation": "auto",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showThresholdLabels": true,
        "showThresholdMarkers": true,
        "sizing": "auto",
        "text": {}
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "# cpu utilzation:\n100 * (1 - avg(rate(node_cpu_seconds_total{mode=\"idle\", job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}[1m])))",
          "legendFormat": "__auto",
          "range": true,
          "refId": "A"
        }
      ],
      "type": "gauge"
    },
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
            "showValues": false,
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
                "value": 0
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
        "h": 7,
        "w": 10,
        "x": 14,
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
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "# cpu utilzation:\n100 * (1 - avg(rate(node_cpu_seconds_total{mode=\"idle\", job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}[1m])))",
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
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "purple",
                "value": 0
              }
            ]
          }
        },
        "overrides": []
      },
      "gridPos": {
        "h": 4,
        "w": 2,
        "x": 8,
        "y": 3
      },
      "id": 18,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "center",
        "orientation": "auto",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "auto",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "disableTextWrap": false,
          "editorMode": "code",
          "expr": "count(count by(cpu) (node_cpu_seconds_total{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}))",
          "fullMetaSearch": false,
          "includeNullMetadata": true,
          "legendFormat": "__auto",
          "range": true,
          "refId": "A",
          "useBackend": false
        }
      ],
      "title": "CPU Cores:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": 0
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 2,
        "x": 4,
        "y": 4
      },
      "id": 36,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "auto",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": " # free\nnode_memory_MemFree_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "RAM Free:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": 0
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 2,
        "x": 6,
        "y": 4
      },
      "id": 32,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "auto",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "node_memory_SwapFree_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "SWAP Free:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "red",
                "value": 0
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 2,
        "x": 4,
        "y": 7
      },
      "id": 35,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "value",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "# used\nnode_memory_MemTotal_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n - node_memory_MemFree_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n - (node_memory_Cached_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n + node_memory_Buffers_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n + node_memory_SReclaimable_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"})",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "RAM Used:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "mappings": [
            {
              "options": {
                "-1": {
                  "index": 0,
                  "text": "No Data"
                }
              },
              "type": "value"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "purple",
                "value": 0
              }
            ]
          },
          "unit": "decbits"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 3,
        "x": 6,
        "y": 7
      },
      "id": 45,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "center",
        "orientation": "auto",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "auto",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "node_network_speed_bytes{device!~\"lo.*|veth.*|docker.*|br.*|virbr.*|tun.*|tap.*|bond.*|dummy.*|sit.*|ip6tnl.*|flannel.*|cali.*|cilium.*|kube.*|weave.*|ovs.*|nlmon.*\", job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"} * 8 ",
          "legendFormat": "__auto",
          "range": true,
          "refId": "A"
        }
      ],
      "title": "Net. Interface Max Speed:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "blue",
                "value": 0
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 3,
        "x": 9,
        "y": 7
      },
      "id": 42,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "text": {
          "percentSize": 1
        },
        "textMode": "value",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "sum(\n  node_network_transmit_bytes_total{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\",\n    device!~\"lo|veth.*|docker.*|br-.*\"\n  }\n)",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "Total Network Transmitted:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "blue",
                "value": 0
              }
            ]
          },
          "unit": "binBps"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 2,
        "x": 12,
        "y": 7
      },
      "id": 29,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "text": {
          "percentSize": 1
        },
        "textMode": "value",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "sum(rate(node_network_transmit_bytes_total{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}[$__rate_interval])*8)",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "Network Tx:",
      "type": "stat"
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
            "showValues": false,
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
                "value": 0
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
        "h": 7,
        "w": 10,
        "x": 14,
        "y": 7
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
      "pluginVersion": "12.4.2",
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
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "blue",
                "value": 0
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 2,
        "x": 4,
        "y": 10
      },
      "id": 37,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "auto",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "# chache + buffer\nnode_memory_Cached_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"} \n+ node_memory_Buffers_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"} \n+ node_memory_SReclaimable_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "RAM Cache:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "description": "since machine was up",
      "fieldConfig": {
        "defaults": {
          "color": {
            "fixedColor": "blue",
            "mode": "thresholds"
          },
          "fieldMinMax": false,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "yellow",
                "value": 0
              }
            ]
          },
          "unit": "dtdhms"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 3,
        "x": 6,
        "y": 10
      },
      "id": 44,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "center",
        "orientation": "vertical",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "auto",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "disableTextWrap": false,
          "editorMode": "code",
          "exemplar": false,
          "expr": "time() - node_boot_time_seconds{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}",
          "fullMetaSearch": false,
          "includeNullMetadata": true,
          "instant": true,
          "intervalFactor": 1,
          "legendFormat": "up_time_in_days",
          "range": false,
          "refId": "A",
          "step": 240,
          "useBackend": false
        }
      ],
      "title": "Up Time:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": 0
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 3,
        "x": 9,
        "y": 10
      },
      "id": 41,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "text": {
          "percentSize": 1
        },
        "textMode": "value",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "sum(\n  node_network_receive_bytes_total{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\",\n    device!~\"lo|veth.*|docker.*|br-.*\"\n  }\n)",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "Total Network Received:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": 0
              }
            ]
          },
          "unit": "binBps"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 2,
        "x": 12,
        "y": 10
      },
      "id": 28,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "text": {
          "percentSize": 1
        },
        "textMode": "value",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "sum(rate(node_network_receive_bytes_total{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}[$__rate_interval])*8)",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "Network Rx:",
      "type": "stat"
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
            "showValues": false,
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
                "value": 0
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
        "h": 9,
        "w": 6,
        "x": 0,
        "y": 13
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
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "# total\nnode_memory_MemTotal_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}",
          "legendFormat": "Total",
          "range": true,
          "refId": "A"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "# used\nnode_memory_MemTotal_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n - node_memory_MemFree_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n - (node_memory_Cached_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n + node_memory_Buffers_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}\n + node_memory_SReclaimable_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"})",
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
          "instant": false,
          "legendFormat": "Cache+Buffer",
          "range": true,
          "refId": "D"
        }
      ],
      "title": "Memory(RAM)",
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
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "index": 0,
                  "text": "No Data"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "purple",
                "value": 0
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 2,
        "x": 6,
        "y": 13
      },
      "id": 39,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "value",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "node_filesystem_size_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\", mountpoint=\"/srv/nfs\", device!~\"rootfs\"}",
          "format": "time_series",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "FS(/srv/nfs) Total",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "red",
                "value": 0
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 2,
        "x": 8,
        "y": 13
      },
      "id": 34,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "value",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "node_filesystem_size_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\", mountpoint=\"/\", fstype!=\"rootfs\"}\n- node_filesystem_avail_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\", mountpoint=\"/\", fstype!=\"rootfs\"}",
          "format": "time_series",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "FS(/) Used:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "red",
                "value": 0
              },
              {
                "color": "#EAB839",
                "value": 5000000000
              },
              {
                "color": "green",
                "value": 10000000000
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 2,
        "x": 10,
        "y": 13
      },
      "id": 12,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "auto",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "auto",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "# used / space:\n#(node_filesystem_size_bytes{instance=\"$node\", mountpoint=\"/\", fstype!=\"rootfs\"}\n#- node_filesystem_avail_bytes{instance=\"$node\", mountpoint=\"/\", fstype!=\"rootfs\"})\n\n# total\n#node_filesystem_size_bytes{instance=\"$node\", mountpoint=\"/\", fstype!=\"rootfs\"}\n\n# free space\nnode_filesystem_avail_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\", mountpoint=\"/\", fstype!=\"rootfs\"}",
          "legendFormat": "__auto",
          "range": true,
          "refId": "A"
        }
      ],
      "title": "FS(/) Free:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "orange",
                "value": 0
              }
            ]
          },
          "unit": "binBps"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 2,
        "x": 12,
        "y": 13
      },
      "id": 31,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "text": {
          "percentSize": 1
        },
        "textMode": "value",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "sum(irate(node_disk_written_bytes_total{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}[1m]))",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "Disk Write:",
      "type": "stat"
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
            "showValues": false,
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
                "value": 0
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
        "h": 8,
        "w": 10,
        "x": 14,
        "y": 14
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
      "pluginVersion": "12.4.2",
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
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "index": 0,
                  "text": "No Data"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "red",
                "value": 0
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 2,
        "x": 6,
        "y": 16
      },
      "id": 38,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "value",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "node_filesystem_size_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\",mountpoint=\"/srv/nfs\", device!~\"rootfs\"}\n- node_filesystem_avail_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\",mountpoint=\"/srv/nfs\", device!~\"rootfs\"}",
          "format": "time_series",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "FS(/srv/nfs) Used:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": 0
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 2,
        "x": 8,
        "y": 16
      },
      "id": 40,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "auto",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "value",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "# used / space:\n#(node_filesystem_size_bytes{instance=\"$node\", mountpoint=\"/\", fstype!=\"rootfs\"}\n#- node_filesystem_avail_bytes{instance=\"$node\", mountpoint=\"/\", fstype!=\"rootfs\"})\n\n# total\n#node_filesystem_size_bytes{instance=\"$node\", mountpoint=\"/\", fstype!=\"rootfs\"}\n\n# free space\nnode_filesystem_avail_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\", mountpoint=\"/srv/nfs\", device!~\"rootfs\"}",
          "legendFormat": "__auto",
          "range": true,
          "refId": "A"
        }
      ],
      "title": "FS(/srv/nfs) Free:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "purple",
                "value": 0
              }
            ]
          },
          "unit": "bytes"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 2,
        "x": 10,
        "y": 16
      },
      "id": 24,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "value",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "node_filesystem_size_bytes{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\",mountpoint=\"/\",fstype!=\"rootfs\"}",
          "format": "time_series",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "FS(/) Total",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "decimals": 0,
          "mappings": [
            {
              "options": {
                "match": "null",
                "result": {
                  "text": "N/A"
                }
              },
              "type": "special"
            }
          ],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "yellow",
                "value": 0
              }
            ]
          },
          "unit": "Bps"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 2,
        "x": 12,
        "y": 16
      },
      "id": 30,
      "maxDataPoints": 100,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "auto",
        "orientation": "horizontal",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "text": {
          "percentSize": 1
        },
        "textMode": "value",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "sum(irate(node_disk_read_bytes_total{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}[1m]))",
          "instant": true,
          "intervalFactor": 1,
          "range": false,
          "refId": "A",
          "step": 240
        }
      ],
      "title": "Disk Read:",
      "type": "stat"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "${DS_PROMETHEUS}"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": 0
              }
            ]
          },
          "unit": "dateTimeAsIso"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 3,
        "w": 8,
        "x": 6,
        "y": 19
      },
      "id": 43,
      "options": {
        "colorMode": "value",
        "graphMode": "none",
        "justifyMode": "center",
        "orientation": "auto",
        "percentChangeColorMode": "standard",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showPercentChange": false,
        "textMode": "auto",
        "wideLayout": true
      },
      "pluginVersion": "12.4.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "# make sure grafana settings->Time options->Time zone is set to Africa/Egypt/Cairo (EEST)\nnode_time_seconds{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"} * 1000\n\n# X make sure grafana settings->Time options->Time zone is set to UTC\n# add machine local time to timezone offset.\n#( sum(node_time_seconds{job=\"$node_exporter_job\", instance=\"$node_exporter_instance\"}) + sum(node_time_zone_offset_seconds{job=\"$node_exporter_job\", #instance=\"$node_exporter_instance\", time_zone=\"EEST\"}) )* 1000",
          "legendFormat": "__auto",
          "range": true,
          "refId": "A"
        }
      ],
      "title": "Machine Local Time:",
      "type": "stat"
    }
  ],
  "preload": false,
  "refresh": "5s",
  "schemaVersion": 42,
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
        "regexApplyTo": "value",
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
        "regexApplyTo": "value",
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
  "title": "Example: Full Machine Metrics AMF95's Edition.",
  "uid": "efdbhz8mybmdcf",
  "version": 9,
  "weekStart": ""
}
```