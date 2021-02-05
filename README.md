# Ansible Role: prometheus-exporter
Installs and configure a generic prometheus exporter given its binary url path.

## Requirements
- [Ansible >= 2.4](https://ansible.com/)

## Role Variables

Please look at the [defaults/main.yml](defaults/main.yml) to see all default variables.

    prometheus_exporter_path: /opt/prometheus-exporters

The directory path to use for install Prometheus exporters.

    prometheus_exporter_list.[].name

Exporter service/binary name to be installed on target machine

    prometheus_exporter_list.[].url

Exporter binary url, compressed or pure binary)

    prometheus_exporter_list.[].unpack

Unpack file if it's compressed

    prometheus_exporter_list.[].args

Exporter arguments

    prometheus_exporter_list.[].config_files.[].src

Local exporter config file path to copy. Not a jinja template.

    prometheus_exporter_list.[].config_files.[].path

Remote exporter config file path

## Example Playbook

```yml
---
- name: My production server
  hosts: all
  vars:
    prometheus_exporter_path: /opt/prometheus-exporters
    prometheus_exporter_list:
      - name: monit
        url: https://github.com/commercetools/monit_exporter/releases/download/v0.0.2/monit_exporter
      - name: process
        url: https://github.com/ncabatoff/process-exporter/releases/download/v0.6.0/process-exporter-0.6.0.linux-amd64.tar.gz
        unpack: true
        unpack_path_tmp: /tmp/process-exporter-0.6.0.linux-amd64/process-exporter
        args: "-config.path=/etc/prom-exporter-process.yml"
        config_files:
          - src: "{{ playbook_dir }}/files/process-exporter.yml"
            path: {{ prometheus_exporter_path }}/-process-exporter.yml
        envs:
          USERNAME=helloworld

  roles:
    - role: prometheus-exporter
      tags: prometheus-exporter
```
