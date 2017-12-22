# Remote Operations Challenge

## Table of Contents

* [Description](#description)
* [Criteria](#criteria)
* [Architecture](#architecture)
* Ansible Roles
    * [Apache](docs/roles/apache.md)
    * [Common](docs/roles/common.md)
    * [Haproxy](docs/roles/haproxy.md)
    * [Nagios Master](docs/roles/nagios_master.md)
    * [Nagios Node](docs/roles/nagios_node.md)
* [Dependencies](#dependencies)
* [Challenges](#challenges)
    * [Nagios](#nagios)
    * [Load Balancing](#load-balancing)
    * [Firewall](#firewall)

### Description

The Remote Operations Challenge is a test designed to evaluate the thought
process of a new hiring candidate. The challenege can best be described as
a simulation of standard DevOps and infrastructure tasks faced in the real
world. The ultimate goal is the management of a four node cluster hosted on
a cloud provider. In this case, the provider is Amazon Web Services, though
this has no bearing on the process.

### Criteria

The cluster is to be configured as follows:

* Two webservers, one serving the single character "a", the other "b"
* One load balancer, servicing the two webserver nodes
    - Any balancing scheme
    - Sticky sessions: a client should hit the same webserver with each request
    - In the event of a node failure, a client should not be rerouted when the original node regains service
    - Accessible on port range 60000:65000
    - Proxies traffic from this port range to port 80 on the webservers
* One Nagios monitoring server
    - Monitors the load balancer and webservers
* All nodes should have a user 'expensify' created
    - User should be in the `sudo` group
    - SSH authentication allowed from the attached `consolidated_keys.txt` (redacted in repo)
* Firewall access configured
    - Only one node should be allowed public SSH access (in this case the Nagios master was chosen)
    - All other traffic locked down as needed

### Architecture

##### Ansible
I chose Ansible for this project for several reasons, the most important being
that I'm very comfortable with it and have plenty of experience wrestling with
it's intricacies. Additionally, the agentless model means that the production
cluster can be deployed with minimal dependencies. The only prior knowledge
needed is the public address of each node. For a scenario such as this, well,
this is all the info we've got so Ansible is perfect!

##### HAProxy
I love HAProxy. It sees heavy use wherever I can implement it, including routing
traffic through my homelab. Configuration is straightforward and powerful, and
tutorials are abundant. HAProxy made me feel like a superhero the first time
I implemented it instead of an AWS ELB.

### Dependencies

This project includes a local test environment with the following dependencies
* vagrant >= 2.0.0
* ansible >= 2.2.0

### Using this Project

`varant up` and you're ready to roll! This will configure a local environment
mirroring the four node Ubuntu 14.04 cluster and automatically provision with
the appropriate Ansible playbooks. Once that is complete, take the cluster for
a spin!
* Access the Nagios dashboard at 172.37.17.54/nagios, username `nagiosadmin`, password `password`
* `curl` some requests to the load balancer at 172.37.17.51:60000-65000
    - Knock a node out of the webserver pool with `vagrant halt ops2` and verify the expected load balancer configuration
* Try and SSH into the other nodes in the cluster, it's a pain! Access `ops1-3` via SSH forwarding through `ops4`.


### Challenges

##### Nagios

Maintaining idempotency through the compilation process for Nagios and assorted
dependencies took quite some time, especially considering this is one of the
first few times I've worked with this suite. Ansible provides a directive
`creates` to note that a raw command creates a file or directory at the noted
path. By manually running through the compilation/installation steps, it was
possible to note exactly what was happening and use these as triggers for
idempotency in the accompanying Ansible tasks. Was this a huge pain and did it
take forever? Yes. Did I get a much better understanding of the installation
process. **Definitely yes.**

Beyond this, the actual configuration of Nagios was mildly difficult as I was
not familiar with the appropriate files and spent quite some time in the docs.
Nagios being the powerhouse that it is, the information was easy to find with
some searching on Stack and various forums.

##### Load Balancing

The requirement to route all traffic from port range 60000:65000 seemed like an
easy task at first, and it was. By telling HAProxy to bind to that port range,
well, that was it. Unfortunately, this caused Ansible to fail when gathering
facts since it had to stat **5000** bound ports before completing a run. That's
no good. Using `iptables` to redirect ports 60001:65000 to port 60000 is a much
lighter method to handle this, and keeps the `haproxy` process bound to a
single port. Of course, this involved dabbling in `iptables` and locking myself
out of the dev environment over and over again...

##### Firewall

Everything that can be said for `iptables` drama can be repeated with `ufw`
configuration. Lots of broken boxes, lots of shattered dreams.

##### Code Review

Team work makes the dream work.
