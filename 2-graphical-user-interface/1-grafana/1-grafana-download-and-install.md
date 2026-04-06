[Web source](https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/)

>**Version control:**
>
>**OS**: Ubuntu 24.4
>
>**grafana**: [grafana-enterprise_12.4.2_linux_amd64.deb](https://dl.grafana.com/grafana-enterprise/release/12.4.2/grafana-enterprise_12.4.2_23531306697_linux_amd64.deb)

---
## (Option_1) Install manually with .deb package:

[Web Source](https://grafana.com/grafana/download?edition=oss)

```bash
sudo apt-get install -y adduser libfontconfig1 musl
wget https://dl.grafana.com/grafana-enterprise/release/12.4.2/grafana-enterprise_12.4.2_23531306697_linux_amd64.deb
sudo dpkg -i grafana-enterprise_12.4.2_23531306697_linux_amd64.deb
```

> Size of .deb `202M`.

**Enable `grafana-server.service`:**
```bash
systemctl enable grafana-server.service
```

**Start `grafana-server.service`:**
```bash
systemctl start grafana-server.service
```

**`grafana-server.service` status:**
```bash
systemctl status grafana-server.service
```

---
## (Option_2) Install using official repository:

**Install the prerequisite packages:**
```bash
sudo apt-get install -y apt-transport-https wget gnupg
```

**Import the GPG key:**
```bash
sudo mkdir -p /etc/apt/keyrings
sudo wget -O /etc/apt/keyrings/grafana.asc https://apt.grafana.com/gpg-full.key
sudo chmod 644 /etc/apt/keyrings/grafana.asc
```

**To add a repository for stable releases, run the following command:**
```bash
echo "deb [signed-by=/etc/apt/keyrings/grafana.asc] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

**To add a repository for beta releases, run the following command:**
```bash
echo "deb [signed-by=/etc/apt/keyrings/grafana.asc] https://apt.grafana.com beta main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

**Run the following command to update the list of available packages:**
```bash
# Updates the list of available packages
sudo apt-get update
```

**To install Grafana Enterprise, run the following command:**
```bash
# Installs the latest enterprise release:
sudo apt-get install grafana-enterprise
 -y
```

> Size is about `202M`.

**Enable `grafana-server.service`:**
```bash
systemctl enable grafana-server.service
```

**Start `grafana-server.service`:**
```bash
systemctl start grafana-server.service
```

**`grafana-server.service` status:**
```bash
systemctl status grafana-server.service
```

## 4. Make sure its working:

>In any web-browser:
>**USER:** admin
>**Password:** admin

```text
http://<MACHINE_IP>:3000
```

**EXP:**
```text
http://localhost:3000
```
