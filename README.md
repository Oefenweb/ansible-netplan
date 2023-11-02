## netplan

[![CI](https://github.com/Oefenweb/ansible-netplan/workflows/CI/badge.svg)](https://github.com/Oefenweb/ansible-netplan/actions?query=workflow%3ACI)
[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-netplan-blue.svg)](https://galaxy.ansible.com/Oefenweb/netplan)

Set up netplan in Debian-like systems.

#### Requirements

* `netplan.io` (will be installed)
* `nplan` (will be installed)

#### Variables

* `netplan_install` [default: `[]`]: Additional packages to install

* `netplan_version` [default: `2`]: Version
* `netplan_renderer` [default: `networkd`]: Renderer

* `netplan_conf_file` [default: `config.yaml`]: Config file name (relative to `netplan_conf_path`)
* `netplan_conf` [default: `''`]: Config in yaml format as a string (to be parsed, formatted and written down to `netplan_conf_file`)
* `netplan_conf_purge` [default: `false`]: Whether or not to purge all "other" config files

## Dependencies

None

#### Example(s)

##### Simple

```yaml
---
- hosts: all
  roles:
    - oefenweb.netplan
```

##### Vagrant

```yaml
---
- hosts: all
  roles:
    - oefenweb.netplan
  vars:
    netplan_conf_file: 01-netcfg.yaml
    netplan_conf: |
      network:
        version: {{ netplan_version }}
        renderer: {{ netplan_renderer }}
        ethernets:
          {{ hostvars[inventory_hostname]['ansible_' + (ansible_interfaces | difference(['lo']) | sort | list | first)]['device'] }}:
            dhcp4: true
          {{ hostvars[inventory_hostname]['ansible_' + (ansible_interfaces | difference(['lo']) | sort | list | last)]['device'] }}:
            addresses:
            - {{ hostvars[inventory_hostname]['ansible_' + (ansible_interfaces | difference(['lo']) | sort | list | last)]['ipv4']['address'] }}/24
    netplan_conf_purge: true
```

#### License

MIT

#### Author Information

Mischa ter Smitten (based on work of [Lucas Harms](https://github.com/lmickh))

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-netplan/issues)!
