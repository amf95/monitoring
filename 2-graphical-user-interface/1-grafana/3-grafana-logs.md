

>**Default location for systemd service logs is in `/var/log/grafana/grafana.log`**

or

```bash
journalctl -xeu grafana-server.service
```

---

>Log level can be set at run time with `--log.level=<SET_LEVEL>` option.
>
>**Debug Levels:** (`debug` > `info` >  `warn` > `error`).
>
>debug = `debug` + `info` +  `warn` + `error`
>
>info     = `info` +  `warn` + `error`
>
>warn   =  `warn` + `error`
>
>error   =`error`

---