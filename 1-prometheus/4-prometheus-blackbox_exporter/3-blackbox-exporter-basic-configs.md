>**Version control:**
>
> **OS**: `Ubuntu 24.04`.
>
>blackbox: [blackbox_exporter-0.28.0.linux-amd64](https://github.com/prometheus/blackbox_exporter/releases/download/v0.28.0/blackbox_exporter-0.28.0.linux-amd64.tar.gz)

---

>Note: If you are getting too much logs on your service side while using root url 
>exp: my-site.com/ then use full path exp: my-site.com/web/login.
>This is caused because of redirection from root/ to /web to /web/login.
## Check yml syntax:

```bash
blackbox_exporter --config.check /etc/blackbox/blackbox.yml
```

---

```bash
vim /etc/blackbox/blackbox.yml
```

## FireWall Config:

```bash
firewall-cmd --permanent --add-port=9115/tcp

firewall-cmd --reload
```

>Note: databases end points are not http so use tcp module.
## Real Server Configs:

```bash
modules:
  http_2xx: # http_2xx could be any name you want 
            # use the same name in prometheus when refering to module:.
    prober: http # http requests.
    timeout: 15s # if target server doesn't respond. 
    # if any of the following configs is note met proper show that endpoint 
    # is down.
    http:  # http configs.
      valid_status_codes: [200, 201, 202, 204]
      follow_redirects: true
# `fail_if_not_ssl: false` will endpoint as UP even if SSL certificate is expired.
      fail_if_not_ssl: false
      ip_protocol_fallback: false # don't switch to ipv6 if ipv4 doesn't work.
      method: GET # default is GET request.
      preferred_ip_protocol: ip4
      #valid_http_versions: 
        #- HTTP/1.1
        #- HTTP/2.0
      tls_config:
        insecure_skip_verify: true #  THIS IGNORES SSL VERIFICATION
#------------------------------------------------------------------------------

  tcp_connect:
    prober: tcp
    timeout: 5s

#------------------------Origrnal Configs Below-------------------------------#
#  http_2xx:
#    prober: http
#    http:
#      preferred_ip_protocol: "ip4"
#  http_post_2xx:
#    prober: http
#    http:
#      method: POST
#  tcp_connect:
#    prober: tcp
#  pop3s_banner:
#    prober: tcp
#    tcp:
#      query_response:
#      - expect: "^+OK"
#      tls: true
#      tls_config:
#        insecure_skip_verify: false
#  grpc:
#    prober: grpc
#    grpc:
#      tls: true
#      preferred_ip_protocol: "ip4"
#  grpc_plain:
#    prober: grpc
#    grpc:
#      tls: false
#      service: "service1"
#  ssh_banner:
#    prober: tcp
#    tcp:
#      query_response:
#      - expect: "^SSH-2.0-"
#      - send: "SSH-2.0-blackbox-ssh-check"
#  ssh_banner_extract:
#    prober: tcp
#    timeout: 5s
#    tcp:
#      query_response:
#      - expect: "^SSH-2.0-([^ -]+)(?: (.*))?$"
#        labels:
#        - name: ssh_version
#          value: "${1}"
#        - name: ssh_comments
#          value: "${2}"
#  irc_banner:
#    prober: tcp
#    tcp:
#      query_response:
#      - send: "NICK prober"
#      - send: "USER prober prober prober :prober"
#      - expect: "PING :([^ ]+)"
#        send: "PONG ${1}"
#      - expect: "^:[^ ]+ 001"
#  icmp:
#    prober: icmp
#  icmp_ttl5:
#    prober: icmp
#    timeout: 5s
#    icmp:
#      ttl: 5
```

