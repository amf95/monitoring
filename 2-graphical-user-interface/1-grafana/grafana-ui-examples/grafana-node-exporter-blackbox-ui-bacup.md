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
      "id": "table",
      "name": "Table",
      "version": ""
    },
    {
      "type": "panel",
      "id": "text",
      "name": "Text",
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
  "editable": true,
  "fiscalYearStartMonth": 0,
  "graphTooltip": 0,
  "id": null,
  "links": [],
  "panels": [
    {
      "gridPos": {
        "h": 2,
        "w": 6,
        "x": 0,
        "y": 0
      },
      "id": 4,
      "options": {
        "code": {
          "language": "plaintext",
          "showLineNumbers": false,
          "showMiniMap": false
        },
        "content": "# **APP: Smartlearning Moodle.**",
        "mode": "markdown"
      },
      "pluginVersion": "11.5.2",
      "type": "text"
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
          "custom": {
            "align": "center",
            "cellOptions": {
              "type": "auto"
            },
            "filterable": false,
            "inspect": false
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
          }
        },
        "overrides": [
          {
            "matcher": {
              "id": "byName",
              "options": "Instance"
            },
            "properties": [
              {
                "id": "links",
                "value": [
                  {
                    "targetBlank": true,
                    "title": "${__data.fields.Instance}",
                    "url": "${__data.fields.Instance}"
                  }
                ]
              },
              {
                "id": "custom.width",
                "value": 296
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Endpoint Status"
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 124
              },
              {
                "id": "thresholds",
                "value": {
                  "mode": "absolute",
                  "steps": [
                    {
                      "color": "red",
                      "value": null
                    },
                    {
                      "color": "green",
                      "value": 1
                    }
                  ]
                }
              },
              {
                "id": "custom.cellOptions",
                "value": {
                  "applyToRow": false,
                  "mode": "gradient",
                  "type": "color-background",
                  "wrapText": false
                }
              },
              {
                "id": "mappings",
                "value": [
                  {
                    "options": {
                      "0": {
                        "index": 0,
                        "text": "Down"
                      },
                      "1": {
                        "index": 1,
                        "text": "UP"
                      }
                    },
                    "type": "value"
                  }
                ]
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Returned Code"
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 118
              },
              {
                "id": "thresholds",
                "value": {
                  "mode": "absolute",
                  "steps": [
                    {
                      "color": "transparent",
                      "value": null
                    },
                    {
                      "color": "green",
                      "value": 200
                    },
                    {
                      "color": "#EAB839",
                      "value": 300
                    },
                    {
                      "color": "orange",
                      "value": 400
                    },
                    {
                      "color": "red",
                      "value": 500
                    }
                  ]
                }
              },
              {
                "id": "custom.cellOptions",
                "value": {
                  "applyToRow": false,
                  "mode": "gradient",
                  "type": "color-background",
                  "wrapText": false
                }
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Has SSL?"
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 100
              },
              {
                "id": "thresholds",
                "value": {
                  "mode": "absolute",
                  "steps": [
                    {
                      "color": "transparent",
                      "value": null
                    },
                    {
                      "color": "green",
                      "value": 1
                    }
                  ]
                }
              },
              {
                "id": "custom.cellOptions",
                "value": {
                  "applyToRow": false,
                  "mode": "gradient",
                  "type": "color-background",
                  "wrapText": false
                }
              },
              {
                "id": "mappings",
                "value": [
                  {
                    "options": {
                      "0": {
                        "index": 0,
                        "text": "NO"
                      },
                      "1": {
                        "index": 1,
                        "text": "Yes"
                      }
                    },
                    "type": "value"
                  }
                ]
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "SSL Version"
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 100
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Time To Expire(Days)"
            },
            "properties": [
              {
                "id": "thresholds",
                "value": {
                  "mode": "absolute",
                  "steps": [
                    {
                      "color": "red",
                      "value": null
                    },
                    {
                      "color": "red",
                      "value": 0
                    },
                    {
                      "color": "yellow",
                      "value": 15
                    },
                    {
                      "color": "orange",
                      "value": 30
                    },
                    {
                      "color": "green",
                      "value": 60
                    },
                    {
                      "color": "blue",
                      "value": 365
                    }
                  ]
                }
              },
              {
                "id": "custom.cellOptions",
                "value": {
                  "mode": "lcd",
                  "type": "gauge",
                  "valueDisplayMode": "color"
                }
              },
              {
                "id": "max",
                "value": 365
              },
              {
                "id": "unit",
                "value": "Days"
              },
              {
                "id": "min",
                "value": 0
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "IP"
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 100
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Last Query Time"
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 175
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Average DNS Lookup"
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 160
              },
              {
                "id": "unit",
                "value": "s"
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Average Probe Lookup "
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 170
              },
              {
                "id": "unit",
                "value": "s"
              }
            ]
          }
        ]
      },
      "gridPos": {
        "h": 10,
        "w": 24,
        "x": 0,
        "y": 2
      },
      "id": 3,
      "options": {
        "cellHeight": "sm",
        "footer": {
          "countRows": false,
          "fields": "",
          "reducer": [
            "sum"
          ],
          "show": false
        },
        "frameIndex": 4,
        "showHeader": true,
        "sortBy": [
          {
            "desc": false,
            "displayName": "IP"
          }
        ]
      },
      "pluginVersion": "11.5.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "probe_success{job=~\"$blackbox_job\", instance=~\"$blackbox_instances\"}",
          "format": "table",
          "instant": true,
          "interval": "",
          "legendFormat": "",
          "refId": "A"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "probe_http_ssl{job=~\"$blackbox_job\", instance=~\"$blackbox_instances\"}",
          "format": "table",
          "instant": true,
          "interval": "",
          "legendFormat": "",
          "refId": "B"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "expr": "(probe_ssl_earliest_cert_expiry{job=~\"$blackbox_job\", instance=~\"$blackbox_instances\"} - time()) / 3600 / 24",
          "format": "table",
          "instant": true,
          "interval": "",
          "legendFormat": "",
          "refId": "C"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "probe_http_status_code{job=~\"$blackbox_job\", instance=~\"$blackbox_instances\"} > 0",
          "format": "table",
          "instant": true,
          "interval": "",
          "legendFormat": "",
          "refId": "D"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "expr": "probe_tls_version_info{job=~\"$blackbox_job\", instance=~\"$blackbox_instances\"}",
          "format": "table",
          "instant": true,
          "interval": "",
          "legendFormat": "",
          "refId": "F"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "avg_over_time(probe_dns_lookup_time_seconds{job=~\"$blackbox_job\", instance=~\"$blackbox_instances\"}[1m])",
          "format": "table",
          "hide": false,
          "instant": true,
          "legendFormat": "",
          "range": false,
          "refId": "E"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "avg_over_time(probe_duration_seconds{job=~\"$blackbox_job\", instance=~\"$blackbox_instances\"}[1m])",
          "format": "table",
          "hide": false,
          "instant": true,
          "legendFormat": "",
          "range": false,
          "refId": "G"
        }
      ],
      "title": "Endpoints HTTP Probe:",
      "transformations": [
        {
          "id": "joinByField",
          "options": {
            "byField": "instance",
            "mode": "outer"
          }
        },
        {
          "id": "organize",
          "options": {
            "excludeByName": {
              "Time 2": true,
              "Time 3": true,
              "Time 4": true,
              "Time 5": true,
              "Time 6": true,
              "Time 7": true,
              "Value #F": true,
              "__name__ 1": true,
              "__name__ 2": true,
              "__name__ 3": true,
              "__name__ 4": true,
              "app 1": true,
              "app 2": true,
              "app 3": true,
              "app 4": true,
              "app 5": true,
              "app 6": true,
              "app 7": true,
              "ip 2": true,
              "ip 3": true,
              "ip 4": true,
              "ip 5": true,
              "ip 6": true,
              "ip 7": true,
              "job 1": true,
              "job 2": true,
              "job 3": true,
              "job 4": true,
              "job 5": true,
              "job 6": true,
              "job 7": true
            },
            "includeByName": {},
            "indexByName": {
              "Time 1": 9,
              "Time 2": 12,
              "Time 3": 15,
              "Time 4": 17,
              "Time 5": 20,
              "Time 6": 33,
              "Time 7": 37,
              "Value #A": 2,
              "Value #B": 4,
              "Value #C": 6,
              "Value #D": 3,
              "Value #E": 7,
              "Value #F": 23,
              "Value #G": 8,
              "__name__ 1": 10,
              "__name__ 2": 13,
              "__name__ 3": 18,
              "__name__ 4": 21,
              "app 1": 24,
              "app 2": 25,
              "app 3": 27,
              "app 4": 29,
              "app 5": 31,
              "app 6": 34,
              "app 7": 38,
              "instance": 0,
              "ip 1": 1,
              "ip 2": 26,
              "ip 3": 28,
              "ip 4": 30,
              "ip 5": 32,
              "ip 6": 35,
              "ip 7": 39,
              "job 1": 11,
              "job 2": 14,
              "job 3": 16,
              "job 4": 19,
              "job 5": 22,
              "job 6": 36,
              "job 7": 40,
              "version": 5
            },
            "renameByName": {
              "Time 1": "Last Query Time",
              "Value #A": "Endpoint Status",
              "Value #B": "Has SSL?",
              "Value #C": "Time To Expire(Days)",
              "Value #D": "Returned Code",
              "Value #E": "Average DNS Lookup",
              "Value #G": "Average Probe Lookup ",
              "__name__ 1": "",
              "instance": "Instance",
              "ip 1": "IP",
              "job 1": "",
              "version": "SSL Version"
            }
          }
        }
      ],
      "type": "table"
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
          "custom": {
            "align": "center",
            "cellOptions": {
              "type": "auto",
              "wrapText": false
            },
            "filterable": false,
            "inspect": false
          },
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": null
              },
              {
                "color": "red",
                "value": 80
              }
            ]
          }
        },
        "overrides": [
          {
            "matcher": {
              "id": "byName",
              "options": "States"
            },
            "properties": [
              {
                "id": "mappings",
                "value": [
                  {
                    "options": {
                      "0": {
                        "color": "red",
                        "index": 1,
                        "text": "Down"
                      },
                      "1": {
                        "color": "green",
                        "index": 0,
                        "text": "UP"
                      }
                    },
                    "type": "value"
                  }
                ]
              },
              {
                "id": "links",
                "value": []
              },
              {
                "id": "custom.width",
                "value": 50
              },
              {
                "id": "custom.cellOptions",
                "value": {
                  "applyToRow": false,
                  "mode": "gradient",
                  "type": "color-background",
                  "wrapText": false
                }
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "RAM Total"
            },
            "properties": [
              {
                "id": "unit",
                "value": "bytes"
              },
              {
                "id": "thresholds",
                "value": {
                  "mode": "absolute",
                  "steps": [
                    {
                      "color": "purple",
                      "value": null
                    }
                  ]
                }
              },
              {
                "id": "custom.width",
                "value": 85
              },
              {
                "id": "custom.cellOptions",
                "value": {
                  "type": "color-text"
                }
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Used RAM"
            },
            "properties": [
              {
                "id": "unit",
                "value": "bytes"
              },
              {
                "id": "custom.width",
                "value": 87
              },
              {
                "id": "custom.cellOptions",
                "value": {
                  "type": "color-text"
                }
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Free RAM"
            },
            "properties": [
              {
                "id": "unit",
                "value": "bytes"
              },
              {
                "id": "thresholds",
                "value": {
                  "mode": "absolute",
                  "steps": [
                    {
                      "color": "transparent",
                      "value": null
                    },
                    {
                      "color": "red",
                      "value": 0
                    },
                    {
                      "color": "#EAB839",
                      "value": 1073741824
                    },
                    {
                      "color": "green",
                      "value": 2147483648
                    }
                  ]
                }
              },
              {
                "id": "custom.width",
                "value": 82
              },
              {
                "id": "custom.cellOptions",
                "value": {
                  "type": "color-background"
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
                "id": "unit",
                "value": "bytes"
              },
              {
                "id": "thresholds",
                "value": {
                  "mode": "absolute",
                  "steps": [
                    {
                      "color": "blue",
                      "value": null
                    }
                  ]
                }
              },
              {
                "id": "custom.width",
                "value": 112
              },
              {
                "id": "custom.cellOptions",
                "value": {
                  "type": "color-text",
                  "wrapText": false
                }
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Instance"
            },
            "properties": [
              {
                "id": "links",
                "value": [
                  {
                    "title": "${__data.fields.Instance}",
                    "url": "${__data.fields.Instance}"
                  }
                ]
              },
              {
                "id": "custom.width",
                "value": 302
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Used SWAP"
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 97
              },
              {
                "id": "custom.cellOptions",
                "value": {
                  "type": "color-text"
                }
              },
              {
                "id": "thresholds",
                "value": {
                  "mode": "absolute",
                  "steps": [
                    {
                      "color": "yellow",
                      "value": null
                    }
                  ]
                }
              },
              {
                "id": "unit",
                "value": "bytes"
              },
              {
                "id": "custom.cellOptions",
                "value": {
                  "type": "color-text",
                  "wrapText": false
                }
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "CPU Average Load"
            },
            "properties": [
              {
                "id": "custom.cellOptions",
                "value": {
                  "mode": "lcd",
                  "type": "gauge",
                  "valueDisplayMode": "color"
                }
              },
              {
                "id": "thresholds",
                "value": {
                  "mode": "percentage",
                  "steps": [
                    {
                      "color": "transparent",
                      "value": null
                    },
                    {
                      "color": "green",
                      "value": 0
                    },
                    {
                      "color": "#EAB839",
                      "value": 10
                    },
                    {
                      "color": "orange",
                      "value": 30
                    },
                    {
                      "color": "red",
                      "value": 50
                    },
                    {
                      "color": "red",
                      "value": 100
                    }
                  ]
                }
              },
              {
                "id": "unit",
                "value": "percent"
              },
              {
                "id": "custom.width",
                "value": 297
              },
              {
                "id": "max",
                "value": 100
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Last Query Time"
            },
            "properties": []
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Cores"
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 60
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Free FS(/)"
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 85
              },
              {
                "id": "unit",
                "value": "decbytes"
              },
              {
                "id": "thresholds",
                "value": {
                  "mode": "absolute",
                  "steps": [
                    {
                      "color": "transparent",
                      "value": null
                    },
                    {
                      "color": "red",
                      "value": 0
                    },
                    {
                      "color": "#EAB839",
                      "value": 500000000
                    },
                    {
                      "color": "green",
                      "value": 10000000000
                    }
                  ]
                }
              },
              {
                "id": "custom.cellOptions",
                "value": {
                  "type": "color-background"
                }
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Free FS(/var)"
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 106
              },
              {
                "id": "unit",
                "value": "decbytes"
              },
              {
                "id": "thresholds",
                "value": {
                  "mode": "absolute",
                  "steps": [
                    {
                      "color": "transparent",
                      "value": null
                    },
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
                }
              },
              {
                "id": "custom.cellOptions",
                "value": {
                  "type": "color-background"
                }
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Kernel Version"
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 114
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Host Name"
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 154
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "IP"
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 100
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "OS Version"
            },
            "properties": [
              {
                "id": "custom.width",
                "value": 92
              }
            ]
          },
          {
            "matcher": {
              "id": "byName",
              "options": "Machine Local Time"
            },
            "properties": [
              {
                "id": "unit",
                "value": "dateTimeAsIso"
              }
            ]
          }
        ]
      },
      "gridPos": {
        "h": 12,
        "w": 24,
        "x": 0,
        "y": 12
      },
      "id": 1,
      "options": {
        "cellHeight": "sm",
        "footer": {
          "countRows": false,
          "enablePagination": false,
          "fields": "",
          "reducer": [
            "sum"
          ],
          "show": false
        },
        "showHeader": true,
        "sortBy": [
          {
            "desc": false,
            "displayName": "IP"
          }
        ]
      },
      "pluginVersion": "11.5.2",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "up{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"}",
          "format": "table",
          "hide": false,
          "instant": true,
          "legendFormat": "__auto",
          "range": false,
          "refId": "A"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "# ram total\nnode_memory_MemTotal_bytes{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"}",
          "format": "table",
          "hide": false,
          "instant": true,
          "legendFormat": "__auto",
          "range": false,
          "refId": "B"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "# used\nnode_memory_MemTotal_bytes{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"}\n - node_memory_MemFree_bytes{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"}\n - (node_memory_Cached_bytes{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"}\n + node_memory_Buffers_bytes{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"}\n + node_memory_SReclaimable_bytes{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"})",
          "format": "table",
          "hide": false,
          "instant": true,
          "legendFormat": "__auto",
          "range": false,
          "refId": "C"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "# free\nnode_memory_MemFree_bytes{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"}",
          "format": "table",
          "hide": false,
          "instant": true,
          "legendFormat": "__auto",
          "range": false,
          "refId": "D"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "# chache + buffer\nnode_memory_Cached_bytes{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"}\n+ node_memory_Buffers_bytes{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"}\n+ node_memory_SReclaimable_bytes{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"}",
          "format": "table",
          "hide": false,
          "instant": true,
          "legendFormat": "",
          "range": false,
          "refId": "E"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "# Sawp usage\nnode_memory_SwapTotal_bytes{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"} - node_memory_SwapFree_bytes{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"}",
          "format": "table",
          "hide": false,
          "instant": true,
          "legendFormat": "",
          "range": false,
          "refId": "F"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "# cpu utilzation\navg by (instance)(100 * (1 - irate(node_cpu_seconds_total{mode=\"idle\", job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"}[1m])))",
          "format": "table",
          "hide": false,
          "instant": true,
          "legendFormat": "",
          "range": false,
          "refId": "G"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "# cpu core count\ncount by (instance)(count by (cpu, instance)(node_cpu_seconds_total{mode=\"idle\", job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"}))",
          "format": "table",
          "hide": false,
          "instant": true,
          "legendFormat": "__auto",
          "range": false,
          "refId": "H"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "disableTextWrap": false,
          "editorMode": "code",
          "exemplar": false,
          "expr": "node_filesystem_avail_bytes{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\", mountpoint=~\"/\", fstype!=\"rootfs\"}",
          "format": "table",
          "fullMetaSearch": false,
          "hide": false,
          "includeNullMetadata": true,
          "instant": true,
          "legendFormat": "__auto",
          "range": false,
          "refId": "I",
          "useBackend": false
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "disableTextWrap": false,
          "editorMode": "code",
          "exemplar": false,
          "expr": "node_filesystem_avail_bytes{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\", mountpoint=~\"/var\", fstype!=\"rootfs\"}",
          "format": "table",
          "fullMetaSearch": false,
          "hide": false,
          "includeNullMetadata": true,
          "instant": true,
          "legendFormat": "__auto",
          "range": false,
          "refId": "J",
          "useBackend": false
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "node_uname_info{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"}",
          "format": "table",
          "hide": false,
          "instant": true,
          "legendFormat": "__auto",
          "range": false,
          "refId": "K"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "${DS_PROMETHEUS}"
          },
          "editorMode": "code",
          "exemplar": false,
          "expr": "node_time_seconds{job=~\"$node_exporter_job\", instance=~\"$node_exporter_instances\"} * 1000",
          "format": "table",
          "hide": false,
          "instant": true,
          "legendFormat": "__auto",
          "range": false,
          "refId": "L"
        }
      ],
      "title": "Machines Summery:",
      "transformations": [
        {
          "id": "joinByField",
          "options": {
            "byField": "instance",
            "mode": "outer"
          }
        },
        {
          "id": "organize",
          "options": {
            "excludeByName": {
              "Time 1": false,
              "Time 10": true,
              "Time 11": true,
              "Time 12": true,
              "Time 2": true,
              "Time 3": true,
              "Time 4": true,
              "Time 5": true,
              "Time 6": true,
              "Time 7": true,
              "Time 8": true,
              "Time 9": true,
              "Value #K": true,
              "__name__ 1": true,
              "__name__ 2": true,
              "__name__ 3": true,
              "__name__ 4": true,
              "__name__ 5": true,
              "__name__ 6": true,
              "app 1": true,
              "app 10": true,
              "app 2": true,
              "app 3": true,
              "app 4": true,
              "app 5": true,
              "app 6": true,
              "app 7": true,
              "app 8": true,
              "app 9": true,
              "cpu": true,
              "device": true,
              "device 1": true,
              "device 2": true,
              "domainname": true,
              "fstype": true,
              "fstype 1": true,
              "fstype 2": true,
              "ip 10": true,
              "ip 2": true,
              "ip 3": true,
              "ip 4": true,
              "ip 5": true,
              "ip 6": true,
              "ip 7": true,
              "ip 8": true,
              "ip 9": true,
              "job 1": true,
              "job 10": true,
              "job 2": true,
              "job 3": true,
              "job 4": true,
              "job 5": true,
              "job 6": true,
              "job 7": true,
              "job 8": true,
              "job 9": true,
              "machine": true,
              "machine_name": true,
              "mode": true,
              "mountpoint": true,
              "mountpoint 1": true,
              "mountpoint 2": true,
              "release": true,
              "sysname": true,
              "version": true
            },
            "includeByName": {},
            "indexByName": {
              "Time 1": 16,
              "Time 10": 46,
              "Time 11": 53,
              "Time 12": 69,
              "Time 2": 20,
              "Time 3": 24,
              "Time 4": 27,
              "Time 5": 31,
              "Time 6": 34,
              "Time 7": 37,
              "Time 8": 38,
              "Time 9": 39,
              "Value #A": 3,
              "Value #B": 4,
              "Value #C": 5,
              "Value #D": 6,
              "Value #E": 7,
              "Value #F": 8,
              "Value #G": 9,
              "Value #H": 10,
              "Value #I": 11,
              "Value #J": 12,
              "Value #K": 60,
              "Value #L": 13,
              "__name__ 1": 17,
              "__name__ 2": 21,
              "__name__ 3": 28,
              "__name__ 4": 40,
              "__name__ 5": 47,
              "__name__ 6": 54,
              "app 1": 18,
              "app 10": 70,
              "app 2": 22,
              "app 3": 25,
              "app 4": 29,
              "app 5": 32,
              "app 6": 35,
              "app 7": 41,
              "app 8": 48,
              "app 9": 55,
              "device 1": 43,
              "device 2": 49,
              "domainname": 56,
              "fstype 1": 44,
              "fstype 2": 50,
              "instance": 0,
              "ip 1": 1,
              "ip 10": 71,
              "ip 2": 61,
              "ip 3": 62,
              "ip 4": 63,
              "ip 5": 64,
              "ip 6": 65,
              "ip 7": 66,
              "ip 8": 67,
              "ip 9": 68,
              "job 1": 19,
              "job 10": 72,
              "job 2": 23,
              "job 3": 26,
              "job 4": 30,
              "job 5": 33,
              "job 6": 36,
              "job 7": 42,
              "job 8": 51,
              "job 9": 57,
              "machine": 58,
              "mountpoint 1": 45,
              "mountpoint 2": 52,
              "nodename": 2,
              "release": 14,
              "sysname": 59,
              "version": 15
            },
            "renameByName": {
              "Time 1": "Last Query Time",
              "Value #A": "States",
              "Value #B": "RAM Total",
              "Value #C": "Used RAM",
              "Value #D": "Free RAM",
              "Value #E": "Cache+Buffer",
              "Value #F": "Used SWAP",
              "Value #G": "CPU Average Load",
              "Value #H": "Cores",
              "Value #I": "Free FS(/)",
              "Value #J": "Free FS(/var)",
              "Value #L": "Machine Local Time",
              "app 9": "",
              "instance": "Instance",
              "ip 1": "IP",
              "nodename": "Host Name",
              "release": "Kernel Version",
              "sysname": "",
              "version": "OS Version"
            }
          }
        }
      ],
      "type": "table"
    }
  ],
  "refresh": "5s",
  "schemaVersion": 40,
  "tags": [],
  "templating": {
    "list": [
      {
        "current": {},
        "definition": "label_values(probe_success{job=\"smartlearning_moodle_blackbox_job\"},job)",
        "includeAll": true,
        "label": "Blackbox Job",
        "multi": true,
        "name": "blackbox_job",
        "options": [],
        "query": {
          "qryType": 1,
          "query": "label_values(probe_success{job=\"smartlearning_moodle_blackbox_job\"},job)",
          "refId": "PrometheusVariableQueryEditor-VariableQuery"
        },
        "refresh": 1,
        "regex": "",
        "sort": 1,
        "type": "query"
      },
      {
        "current": {},
        "definition": "label_values(up{app=\"moodle_smartlearning\", job=\"moodle_smartlearning_node_exporter_job\"},job)",
        "includeAll": true,
        "label": "Node Exporter Job",
        "multi": true,
        "name": "node_exporter_job",
        "options": [],
        "query": {
          "qryType": 1,
          "query": "label_values(up{app=\"moodle_smartlearning\", job=\"moodle_smartlearning_node_exporter_job\"},job)",
          "refId": "PrometheusVariableQueryEditor-VariableQuery"
        },
        "refresh": 1,
        "regex": "",
        "sort": 1,
        "type": "query"
      },
      {
        "allValue": ".+",
        "current": {},
        "definition": "label_values(probe_success{app=\"smartlearning_moodle\"},instance)",
        "includeAll": true,
        "label": "Blackbox Instance",
        "multi": true,
        "name": "blackbox_instances",
        "options": [],
        "query": {
          "qryType": 1,
          "query": "label_values(probe_success{app=\"smartlearning_moodle\"},instance)",
          "refId": "PrometheusVariableQueryEditor-VariableQuery"
        },
        "refresh": 1,
        "regex": "",
        "sort": 7,
        "type": "query"
      },
      {
        "allValue": ".+",
        "current": {},
        "definition": "label_values(up{app=\"moodle_smartlearning\", job=\"moodle_smartlearning_node_exporter_job\"},instance)",
        "includeAll": true,
        "label": "Node Exporter Instance",
        "multi": true,
        "name": "node_exporter_instances",
        "options": [],
        "query": {
          "qryType": 1,
          "query": "label_values(up{app=\"moodle_smartlearning\", job=\"moodle_smartlearning_node_exporter_job\"},instance)",
          "refId": "PrometheusVariableQueryEditor-VariableQuery"
        },
        "refresh": 1,
        "regex": "",
        "sort": 7,
        "type": "query"
      }
    ]
  },
  "time": {
    "from": "now-30m",
    "to": "now"
  },
  "timepicker": {},
  "timezone": "Africa/Cairo",
  "title": "Production: moodle_smartlearning dashboard",
  "uid": "eewyf2sr28sg0e",
  "version": 31,
  "weekStart": ""
}
```