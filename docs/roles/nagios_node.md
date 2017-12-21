# Ansible Role - Nagios Node

Configures NRPE on monitored nodes

### Variables

None

### Tasks

* `Install Nagios Plugins and NRPE` - Installs plugins and NRPE
* `Configure NRPE` - Configures NRPE and custom commands

### Templates

None

### Handlers

* `restart nrpe` - Restart nagios-nrpe-server service
