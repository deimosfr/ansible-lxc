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

```
    # The DNS domain where LXC containers should belong to
    lxc_domain: mydomain.lan
    # The subnet for DHCP server served by dnsmasq
    lxc_ipnet: 192.168.0.0/24
    # The DHCP range served by dnsmasq with the lease time
    lxc_dhcp_range: 192.168.0.50,192.168.0.200,1h
    # Set network mode: nat or bridge
    lxc_network_mode: lxc-net
```

You can also set specific parameters depending the host configuration you want
to give. Here is an example

```
[servers]
bridge.server.lan lxcbr0_ip=10.0.0.1 lxc_ipnet=10.0.0.0/24 lxc_dhcp_range=10.0.0.200,10.0.0.220,1h lxc_network_mode=bridge
nat.server.lan lxcbr0_ip=192.168.0.1 lxc_ipnet=192.168.0.0/24 lxc_dhcp_range=192.168.0.200,192.168.0.220,1h lxc_network_mode=lxc-net
```

Network modes
-------------

This roles setup network for containers in one of the following mode:

- `lxc-net`: setup `lxc-net` service for NAT, DHCP and DNS resolution.
- `nat`: Create a dedicated `lxcbr0` in `/etc/network/interfaces.d` and setup
  NAT with `iptables`. With `lxc_tight_dedicated_dnsmasq: yes`, you have a
  dedicated DHCP server for your containers. This is an Ansible implementation
  of `lxc-net` service.
- `bridge`: takes control of `/etc/network/interfaces`, creates a `lxcbr0`
  bridge with `eth0` in. This allow to share containers to the world, without
  NAT.


DNS resolution for containers
-----------------------------

Having DNS resolution of container is very confortable. The idea is to have a
`dnsmasq` running on host with a DNS delegation for `.lxc` domains to a
dedicated `dnsmasq` instance managing containers IP and names.

A wildcard record is registerd for the host. All containers can resolve host IP
in container network with e.g. `lxchost.lxc`.


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
    - lxc_domain: lxc
    - lxc_ipnet: 192.168.0.0/24
    - lxc_dhcp_range: 192.168.0.50,192.168.0.200,1h
    - lxcbr0_ip: 192.168.0.1
    - lxcbr0_netmask: 255.255.255.0
    - lxc_network_mode: lxc-net
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


