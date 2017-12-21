# Ansible Role - Common

Common tasks, including user creation and firewall management.

### Variables

* `ops_user` - Defined in `<inventory>/group_vars/all`. Configuration of project user
* `firewall_rules` - Undefined. Additional UFW rules.
* `ipables_tules` - Defined in `<inventory>/group_vars/balancers`. Custom IP tables rule for routing port range to single port

### Tasks

* `User tasks` - Manage the creation and configuration of project user
* `Manage UFW` - Manages UFW rules
* `Manage iptables` - Manages iptables rules

### Templates

None

### Handlers

None
