Network-configure
=========

This role is a quick proof of concept to test the layout of systemd-networkd and how we can take generic type of data. It hasn't been tested properly
Requirements
------------

This module requires Ansible 2.4

Role Variables
--------------

See defaults for variables and descriptions

## Usage


The following variable is crucial:

`network_configure_interfaces` - This variable drives the network data in this role.


```
network_configure_interfaces:
  primary:                       # Friendly name - Also used for naming the network unit file. Every one of these will be configured as its own unit file
    device_type: [ eth ]         # Device type - Currently unused in this role, to be used in future
    network_order: 1             # Ordering - Currently unused in this role, to be used in future
    config_Match:                # Anything pre-pended with config_ will be used as a unit title [Match] in this case
      Name: enp0s3               # Anything under a config_ will be used as a Key:Value pair, so this is converted to Name=enp0s3 under [Match]
    config_Network:              # Similar to above, but now [Network]
    Address: 10.0.3.25/24        # Similar to above, Address=10.0.3.25/24
    DNS: [ 8.8.8.8, 8.8.4.4 ]    # If any of these items are a list each entry will be repeated, so two DNS=8.8.8.8 and DNS=8.8.4.4 in this case
  secondary:
    device_type: [ eth ]
    network_order: 2
    config_Match:
      Name: enp0s8
    config_Network:
      DHCP: ipv4

```

Example Playbook
----------------

Example to call:

    - hosts: all
      roles:
         - { role: network-configure }
