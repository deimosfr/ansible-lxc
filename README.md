Ansible LXC playbook
=====

This role installs and configures LXC on a server. The user can specify
dnsmasq IP parameters for DNS and DHCP purpose. This playbook is only
compatible for Debian.

Requirements
------------

This role requires Ansible 1.4 or higher and platform requirements are listed
in the metadata file.

Role Variables
--------------

The variables that can be passed to this role and a brief description about
them are as follows.

    # The DNS domain where LXC containers should belong to
    lxc_domain: mydomain.lan
    # The subnet for DHCP server served by dnsmasq
    lxc_ipnet: 192.168.0.0/24
    # The DHCP range served by dnsmasq with the lease time
    lxc_dhcp_range: 192.168.0.50,192.168.0.200,1h

Examples
========

```
# Roles
- name: deploy lxc
  hosts: servers
  user: root

  roles:
    - lxc

  vars:
    - lxc_template_path: /tmp/lxc-debian-wheezy-template
    - lxc_domain: mydomain.lan
    - lxc_ipnet: 192.168.0.0/24
    - lxc_dhcp_range: 192.168.0.50,192.168.0.200,1h
```

Dependencies
------------

None

License
-------

GPL

Author Information
------------------

Pierre Mavro / deimosfr


