**Check if yaml is ok:**
```bash
ansible-playbook --syntax-check -i hosts node_exporter_playbook.yml
```

**List tasks:**
```bash
ansible-playbook -i hosts node_exporter_playbook.yml --list-tasks
```

**RUN:**
```bash
ansible-playbook -i hosts node_exporter_playbook.yml
```

> `-i`: inventory.

---

**Check if machines can be connected to:**
```bash
ansible -i hosts moodle_ubuntu_targets -m ping
```

> `-m`: module.

---

**If you are gonna use passwords create this `ansible.cfg` file in your working directory:**

```bash
vim ansible.cfg
```

**Add the following:**
```bash
[defaults]
host_key_checking = False
```

---

**Create hosts file:**
```bash
vim hosts
```

> `ubuntu_targets` is the groups name.

**Add the following for example:**
```bash
[ubuntu_targets]
server_1 ansible_host=10.0.0.76 ansible_user=root ansible_password=myPassword
server_2 ansible_host=10.0.0.71 ansible_user=root ansible_password=myPassword

[redhat_targets]
server_a ansible_host=10.0.0.86 ansible_user=root ansible_password=myPassword
server_b ansible_host=10.0.0.81 ansible_user=root ansible_password=myPassword
```

---

```bash
vim node_exporter_playbook.yml
```

> **change `ubuntu_targets`.**

**Add the follwing:**
```YAML
---
- name: Install and configure Prometheus Node Exporter
  hosts: ubuntu_targets
  become: true

  vars:
    node_exporter_version: "1.11.1"
    node_exporter_user: "node_exporter"
    node_exporter_install_dir: "/opt/monitoring/node_exporter"
    node_exporter_log_dir: "/opt/monitoring/node_exporter/log"
    node_exporter_archive: "node_exporter-{{ node_exporter_version }}.linux-amd64.tar.gz"
    node_exporter_tmp_dir: "/tmp"
    node_src_exporter_path: "."

  tasks:
    - name: Ensure required directories exist
      file:
        path: "{{ item }}"
        state: directory
        mode: "0755"
      loop:
        - /opt/monitoring
        - "{{ node_exporter_install_dir }}"
        - "{{ node_exporter_log_dir }}"

    - name: Copy Node Exporter archive to server
      copy:
        src: "{{ node_src_exporter_path }}/{{ node_exporter_archive }}"
        dest: "{{ node_exporter_tmp_dir }}/{{ node_exporter_archive }}"
        mode: "0644"

    - name: Extract Node Exporter archive
      unarchive:
        src: "{{ node_exporter_tmp_dir }}/{{ node_exporter_archive }}"
        dest: "{{ node_exporter_tmp_dir }}"
        remote_src: true
        creates: "{{ node_exporter_tmp_dir }}/node_exporter-{{ node_exporter_version }}.linux-amd64"

    - name: Copy Node Exporter files to installation directory
      copy:
        src: "{{ node_exporter_tmp_dir }}/node_exporter-{{ node_exporter_version }}.linux-amd64/"
        dest: "{{ node_exporter_install_dir }}/"
        remote_src: true
        mode: "0755"

    - name: Create node_exporter system user
      user:
        name: "{{ node_exporter_user }}"
        shell: /bin/false
        create_home: false
        system: true

    - name: Set ownership on Node Exporter directory
      file:
        path: "{{ node_exporter_install_dir }}"
        owner: "{{ node_exporter_user }}"
        group: "{{ node_exporter_user }}"
        recurse: true

    - name: Create systemd service file
      copy:
        dest: /etc/systemd/system/node_exporter.service
        mode: "0644"
        content: |
          [Unit]

          Description="node_exporter extracts system metrics and reports it to prometheus. Default web ui http://localhost:9100/metrics"

          Wants=network-online.target

          After=network-online.target

          # wait x seconds between each time you try to restart on-failure.
          StartLimitIntervalSec=30

          # try to restart service x times on-failure
          StartLimitBurst=3  

          [Service]

          User=node_exporter

          Group=node_exporter

          Type=simple

          ExecStart=/usr/local/bin/node_exporter \
          #	--collector.systemd \
            --web.listen-address=':9100' \
            --log.level=warn
          #    --web.config.file="/etc/node_exporter/web-config.yml"

          # web-config.yml contains TLS configs.

          Restart=on-failure

          # node-exporter only outputs one file for all logs(info, warn, error,...). 
          StandardError=append:/var/log/node_exporter/error.log

          [Install]
          WantedBy=multi-user.target

    - name: Reload systemd daemon
      systemd:
        daemon_reload: true

    - name: Enable and start Node Exporter
      systemd:
        name: node_exporter
        enabled: true
        state: started

    # - name: Open firewall for Node Exporter
    #   block:
    #     - name: Open port on Ubuntu (ufw)
    #       ufw:
    #         rule: allow
    #         port: 9100
    #         proto: tcp
    #       when: ansible_os_family == "Debian"

    #     - name: Open port on RedHat (firewalld)
    #       firewalld:
    #         port: 9100/tcp
    #         permanent: true
    #         state: enabled
    #         immediate: yes
    #       when: ansible_os_family == "RedHat"

```