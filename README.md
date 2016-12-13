gchiesa.swarm
=================

Install and setup a swarm cluster on Oracle Linux 7

Role Variables
--------------
```
# proxy server to use for yum installation
proxy: ""

# consul hostname to use for registering
# it will be randomly choosen from the consul_cluster host group
consul_hostname: "{{ groups['consul_cluster'] | random | string }}"

# iptables configuration file
iptables_config: "/etc/sysconfig/iptables"
```
The cluster is built based on the existence of hosts in two specific hostgroups:

__swarm_managers__ : should contain the manager nodes
__swarm_agents__ : should contain all the node that should be part of the swarm cluster

Dependencies
------------

This role depends on ```gchiesa.consul``` since it uses consul as discovery service

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - { role: gchiesa.swarm }

The inventory should mention the groups for consul_cluster and swarm_nodes and managers. Check the example inventory below:

```
[targets:vars]
ansible_host=localhost  
ansible_become=true  
ansible_ssh_user=vagrant  
ansible_ssh_private_key_file=~/.vagrant.d/insecure_private_key  
proxy=

[consul_cluster:vars]
consul_bootstrap_hostname=test01.local  

[targets]
test01.local ansible_port=2200  
test02.local ansible_port=2201  
test03.local ansible_port=2202  
test04.local ansible_port=2203  

[consul_cluster]
test01.local  
test02.local  
test03.local  

[swarm_agents]
test01.local  
test02.local  
test03.local  
test04.local  

[swarm_managers]
test01.local  
test02.local  
```

License
-------

BSD
