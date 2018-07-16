# ICN Testbed
Ansible playbooks for maintaining the ICN testbed

## ICN testbed Ansible documentation
coming soon.

## ICN testbed network design
ICN nodes are standard x86 servers that run open source software and are used to forward ICN traffic between each other.
DPDK-compatible NICs are the only hardware requirement for an ICN node.
All ICN nodes use VLANs (via AL2S) to communicate with each other.
There are two types of VLANs that are used:

Data VLANs:: 
These VLANs are used to tranport ICN traffic (e.g., CCN packets) between nodes.
They are used to build a star topology, where edge nodes are hosted at an institution's site and connect to (one or more) central node(s) hosted within the Internet2 network.
This design helps the testbed scale and avoids the need for a full-mesh of VLANs. 
Management VLAN::
This VLAN is used to connect an ICN node to a management node. 
The management node hosts software for node automation (Ansible) and acts as a proxy to the public internet for software updates.

## Adding ICN nodes to the testbed
Before being added to the ICN network, an ICN node MUST contain the following:

* Ubuntu 16.04
* Private IP address assigned to management interface that connects to the ICN Management VLAN
* Provisioning username/password: icn/icn
* DPDK-compatible NIC

### Preparation
Tasks performed on management node before a new node is added:

* Information to-be collected from participant:

  Institution, contact, email, participant's username, installation username & password
  
  Participant's SSH key (roles/users/files/keys/<ORG>/<ORG>.sshkeys)
    
    place the ssh keys in home/${username}/ssh-keys
  
* Update ansible database (/hosts):

  Username, hostname, locations (geo), v4 address, v6 address
  
  Add new node to DNS server
  
  Update hostkey file (roles/etcfiles/files/ssh_known_hosts)
  
  Update user file (roles/users/vars/ICN-users.yml)

### Ansible workflow

* Perform a bunch of pre-tasks
** setup proxy configuration, upgrade box, check if machine has already been provisioned, ping tests, check architecture, check ubuntu version
* etcfiles
** update hostname, timezone, ssh_known_hosts
* apt
** auto upgrade settings, setup LXC repo
* users
** add users to node (admin vs. user), set permissions, add .ssh dir, add ssh keys, clean up provisioning user
