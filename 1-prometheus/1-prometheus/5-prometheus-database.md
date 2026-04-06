
> **Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
> **Prometheus**: [prometheus-3.10.0.linux-amd64](https://github.com/prometheus/prometheus/releases/download/v3.10.0/prometheus-3.10.0.linux-amd64.tar.gz)

---

[source](https://prometheus.io/docs/prometheus/latest/storage/)

## Database Type: 

>Prometheus uses a custom time series database(tsdb).

## Data Retention:

>- Prometheus retains data for a configurable amount of time, which is set using the `--storage.tsdb.retention.time` flag (default is 15 days).
>- Prometheus retains data for a configurable size, which is set using the `--storage.tsdb.retention.size` flag (default is 0 = disabled).
>- Older data is automatically deleted when it exceeds the retention period.

**EXP:**
```bash
prometheus --storage.tsdb.retention.time=30d # Units supported: y, w, d, h, m, s, ms.
prometheus --storage.tsdb.retention.size=512MB # Units supported: B, KB, MB, GB, TB, PB, EB.
```

## Database Location:

> location is determined at app start with --storage.tsdb.path flag.

**EXP:**
```
prometheus --storage.tsdb.path="/var/lib/prometheus/" 
```


## Data Structure:

>- Prometheus uses a custom time-series database (TSDB) for storing metrics.
>- The data is stored in a series of blocks, each containing a chunk of time-series data.
>- Each block is stored in a separate directory under the `data` directory, organized by time ranges.
>- The `wal` (Write-Ahead Log) directory stores recent data that hasn't yet been compacted into blocks.

**EXP:**
```
data/
├── 01BKGTZQ1SYQJTR4PB43C8PD98/
├── 01BKGTZQ1HHWHV8FBJXW1Y3W0K/
└── wal/
```
