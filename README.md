# iptables

Ansible role to configure firewall rules with iptables on Debian and Ubuntu
platforms.
Rules are associated to a network interface and performed on specific events
(``down``, ``post-down``, ``pre-up`` and ``up``, see the respective folders
in ``/etc/network/``).

Minimum Ansible Version: 1.8

## Systems supported

* Debian
    - Squeeze   (6)
    - Wheezy    (7)
    - Jessie    (8)
* Ubuntu
    - Precise   (12.04)
    - Trusty    (14.04)

## Example (Playbook)

```yaml
- name: MyHost
  hosts: my_host
  roles:
    - iptables
  vars:
    - iptables_rules:
        - interface: eth0
          event: up
          rules:
              - name: Flush the NAT table to avoid duplicated rules
                lines:
                    - "iptables -t nat -F"
              - name: ssh
                lines:
                    - "iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 22345 -j DNAT --to-destination 172.24.99.10:22"
                    - "iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 22346 -j DNAT --to-destination 172.24.99.11:22"
              - name: postgresql
                lines:
                    - "iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 5432 -j DNAT --to-destination 172.24.99.10:5432"
```

## Parameters

```yaml
- iptables_rules:
    - interface: eth0
      event: up
      rules:
            - name: NO RULE DEFINED
              lines: []
```
