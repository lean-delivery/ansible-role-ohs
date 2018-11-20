ansible-role-ohs
=========

[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-ohs.svg?branch=develop)](https://travis-ci.org/lean-delivery/ansible-role-ohs) [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)


## Summary

This role installs Oracle HTTP server 11.x on linux platforms.

Requirements
------------

Only Centos 6.x

On Centos 7.x getting exception: "Problem: This Oracle software is not certified on the current operating system. Recommendation: Make sure you are installing the software on the correct platform. Warning: Check:CertifiedVersions failed."

Minimal Ansible version: 2.5

Note: To install OHS server into AWS instance there must be root volume size at least 10GB. Or you may mount additional partition into {{ base_root }} 
--------------

Role Variables
--------------

# Defaults file for ohs
ohs_install_archive: "http://example.com/ofm_webtier_linux_11.1.1.7.0_64_disk1_1of1.zip"

# Service name
ohs_service: "ohs"

# OHS user and group
ohs_user: "oracle"
install_group: "oinstall"

# Base directory for OHS installation
base_root: "/opt"

# Oracle Inventory directory
inventory_directory: "{{ base_root }}/oraInventory"

# Oracle installation file with stored variables with oinstall group and inventory directory
ora_inst: "/etc/oraInst.loc"

# Oracle Middleware home
middleware_home: "{{ base_root }}/oracle/mw"

# Oracle Home
ohs_oracle_home: "{{ middleware_home }}/Oracle_WT1"

# OHS instance name
instance_name: "instance1"

# OHS instance home dir
instance_home: "{{ ohs_oracle_home }}/instances/{{ instance_name }}"

# OHS component name
ohs_component_name: "ohs1"

# OHS port number
ohs_port: 7777

# Path to swap
swapfile_path: "/swapfile"

# dd Block size
swapfile_bs_size_mb: "1"

# Size of swap file
swapfile_count: "{{ 16384 if ansible_memtotal_mb > 10240 else (ansible_memtotal_mb * 1.5)|
                  round|int if ansible_memtotal_mb > 2048 else 8192 }}"

# To pass gitlab-ci with docker. Due to issue with swap unmount during docker container deletion
docker_play: False


## SELinux
------------
No problems in role with active SELinux were encountered. In a case of any issues you should [disable SELinux Temporarily or Permanently](https://www.tecmint.com/disable-selinux-temporarily-permanently-in-centos-rhel-fedora/).
You can use some steps as example based on [elk5-nginx](https://git.epam.com/epm-ldi/elk5-nginx/blob/master/tasks/selinux-elk5-nginx.yml) or [zabbix-server](https://git.epam.com/epm-ldi/zabbix-server/blob/master/tasks/selinux-zabbix-server.yaml) implementations.

## Administration Console and Enterprise Manager

Validate Administration Console and Enterprise Manager through both Oracle HTTP Server instances using the following URL:

	https://hostname:7777


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    ```yaml
    - name: "Install OHS"
      hosts: ohs
      roles:
        - role: lean_delivery.ohs



To avoid error "Failed to set permissions on the temporary files Ansible needs to create when becoming an unprivileged user. Operation not permitted. For information on working around this, see https://docs.ansible.com/ansible/become.html#becoming-an-unprivileged-user" add parameter:

	ansible_ssh_pipelining: true


License
-------
Apache

Author Information
------------------

authors:
  - Lean Delivery Team <team@lean-delivery.com>