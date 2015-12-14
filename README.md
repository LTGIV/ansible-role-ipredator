Role Name
=========

Role by Louis T. Getterman IV for OpenVPN client using IPredator service to assist in network diagnostics.

Requirements
------------

* Debian Family (e.g. Ubuntu)
* RedHat Family (e.g. CentOS)

Role Variables
--------------

* target : set with --extra-vars 'target=InventoryHost'
* openvpn_owner : set with --extra-vars 'openvpn_owner=user'
* openvpn_group : set with --extra-vars 'openvpn_group=group'

* openvpn_path : Typically, it's '/etc/openvpn'

* openvpn_min_tls12 : Is TLS 1.2 supported as a minimum?
* openvpn_install_start : Start VPN after finishing installing?

* openvpn_route_address: Used for overriding default address.
* openvpn_route_network: Used for overriding default network.
* openvpn_route_netmask: Used for overriding default netmask.
* openvpn_route_cidr: Used for overriding CIDR calculated from netmask (default netmask, or netmask override).
* openvpn_route_if: Used for overriding network interface.
* openvpn_route_gateway: Used for overriding gateway.

* VPN scripts
  * Entries
    * openvpn_if_up - Interface Up
    * openvpn_if_down - Interface Down
    * openvpn_route_up - Route Up
  * List comprised of two items:
    * Source
      * (blank) - used if you need to specify already-existing program (e.g. /etc/openvpn/update-resolv-conf on Debian-based OS)
      * template - Generic starter script for Interface Up/Down; and more intricate script for Route Up, which is based upon openvpn_route_* information.
      * script path on host machine
    * Remote path to refer (and optionally, save) to

Dependencies
------------

(None)

Example Playbook (for Ubuntu 14)
----------------

```
---
#
# Example Usage:
# [user@host ~$] ansible-playbook playbook.yml --extra-vars 'target=InventoryHost'
#

- hosts: '{{ target }}'
  sudo: yes

  vars_prompt:
    - name: "ipredator_username"
      prompt: "Enter IPredator username"
      private: no

    - name: "ipredator_password"
      prompt: "Enter IPredator password"
      private: yes

  roles:
  - { # IPredator
      role: ltgiv.ipredator,
      #----- Variables
      # Configuration
      openvpn_path: '/etc/openvpn',
      openvpn_owner: root,
      openvpn_group: root,
      openvpn_min_tls12: no,
      openvpn_install_start: no,

      # Network
      openvpn_route_if: eth0,
      openvpn_route_address: 192.168.1.10,
      openvpn_route_netmask: 255.255.255.0,
      openvpn_route_gateway: 192.168.1.1,
      openvpn_route_network: 192.168.1.0,

      # Scripts
      openvpn_if_up: ['', '/etc/openvpn/update-resolv-conf'],
      openvpn_if_down: ['', '/etc/openvpn/update-resolv-conf'],
      openvpn_route_up: ['template', '/usr/local/bin/vpn-route-up'],
      #-----/Variables
    }
```

License
-------

CC-BY-SA

Author Information
------------------

* [Louis T. Getterman IV](https://github.com/LTGIV)
