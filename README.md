ansible-role-ohs
=========

[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-ohs/master/LICENSE)
[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-ohs.svg?branch=develop)](https://travis-ci.org/lean-delivery/ansible-role-ohs)
[![Build Status](https://gitlab.com/lean-delivery/ansible-role-ohs/badges/master/build.svg)](https://gitlab.com/lean-delivery/ansible-role-ohs)
[![Galaxy](https://img.shields.io/badge/galaxy-lean__delivery.ohs-blue.svg)](https://galaxy.ansible.com/lean_delivery/ohs)
![Ansible](https://img.shields.io/ansible/role/d/role_id.svg)
![Ansible](https://img.shields.io/badge/dynamic/json.svg?label=min_ansible_version&url=https%3A%2F%2Fgalaxy.ansible.com%2Fapi%2Fv1%2Froles%2Frole_id%2F&query=$.min_ansible_version)

Summary
-------

This role installs Oracle HTTP server 11.x on linux platforms.

Requirements
------------

Only Centos 6.x

On Centos 7.x getting exception: "Problem: This Oracle software is not certified on the current operating system. Recommendation: Make sure you are installing the software on the correct platform. Warning: Check:CertifiedVersions failed."

Minimal Ansible version: 2.5

#### **NOTE**:  
To install OHS server into AWS instance there must be root volume size at least 10GB. Or you may mount additional partition into {{ base_root }} 

Role Variables
--------------

- `ohs_install_archive` OHS installation archive path  
  default `http://example.com/ofm_webtier_linux_11.1.1.7.0_64_disk1_1of1.zip`

- `base_root` Base directory path for OHS installation  
  default: `"/opt"`

- `instance_name` OHS instance name  
  default: `"instance1"`

- `ohs_component_name` OHS component name  
  default: `"ohs1"`

- `ohs_user` Run ohs on behalf of user  
  default: `"oracle"`

- `ohs_service` Service name  
  default: `"ohs"`

- `middleware_home` Oracle middleware home  
  default: `"{{ base_root }}/oracle/mw"`

- `ohs_oracle_home` Oracle OHS home dir  
  default: `"{{ middleware_home }}/Oracle_WT1"`

- `instance_home` Oracle OHS instance home  
  default `"{{ ohs_oracle_home }}/instances/{{ instance_name }}"`

- `ohs_hostname` OHS hostname  
  default: `"{{ ansible_fqdn }}"`

- `ohs_port` OHS port number  
  default: `7777`

- `swapfile_path` swapfile  
  default: `"/swapfile"`

- `swapfile_bs_size_mb` dd block size to be copied for one time  
  default: `"1"`

- `swapfile_count` set default `count=` value for `dd` for copies only this number of blocks  
  default: `{{ 16384 if ansible_memtotal_mb > 10240 else (ansible_memtotal_mb * 1.5) |
            round|int if ansible_memtotal_mb > 2048 else 8192 }}`

- `install_group` Oracle Universal Installer group  
  default: `"oinstall"`

- `inventory_directory` Oracke inventory directory  
  default: `"{{ base_root }}/oraInventory"`

- `ora_inst` Oracle inventory and installation group location file  
  default: `"/etc/oraInst.loc"`

SELinux
------------
No problems in role with active SELinux were encountered. In a case of any issues you should [disable SELinux Temporarily or Permanently](https://www.tecmint.com/disable-selinux-temporarily-permanently-in-centos-rhel-fedora/).
You can use some steps as example based on [elk5-nginx](https://git.epam.com/epm-ldi/elk5-nginx/blob/master/tasks/selinux-elk5-nginx.yml) or [zabbix-server](https://git.epam.com/epm-ldi/zabbix-server/blob/master/tasks/selinux-zabbix-server.yaml) implementations.

Administration Console and Enterprise Manager
---------------------------------------------

Validate Administration Console and Enterprise Manager through both Oracle HTTP Server instances using the following URL:
  ``` 
  https://hostname:7777 
  ```

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- name: "Install OHS"
  hosts: ohs
  roles:
    - role: lean_delivery.ohs
```

#### **NOTE**: 
To avoid error "Failed to set permissions on the temporary files Ansible needs to create when becoming an unprivileged user. Operation not permitted. For information on working around this, see https://docs.ansible.com/ansible/become.html#becoming-an-unprivileged-user add parameter:

```ansible_ssh_pipelining: True```

License
-------
Apache

Author Information
------------------

authors:
  - Lean Delivery Team <team@lean-delivery.com>
