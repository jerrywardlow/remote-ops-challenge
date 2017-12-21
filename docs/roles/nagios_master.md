# Ansible Role - Nagios Master

Manages configuration for Nagios master nodes

### Variables

* `nagios` - Defined in `<inventory>/group_vars/nagios_masters`. Configuration values for Nagios, Plugins, and NRPE, including versions and prerequisites.
* `nagios.htpasswd` - Defined in `<inventory>/group_vars/nagios_masters`. Defines a list of `htpasswd` users and passwords which can be used to access the Nagios dashboard.

### Tasks

* `prerequisites` - Manages Nagios prerequisites and dependencies
* `install_core` - Installs the core Nagios suite
* `install_nrpe` - Installs the `check_nrpe` plugin
* `install_plugins` - Installs Nagios plugins
* `configure` - Manages Nagios and dependent configurations

### Templates

* `node.cfg.j2` - Template for the creation of config files for monitored nodes

### Handlers

* `restart apache2` - Restart Apache2 service
* `restart nagios` - Restart Nagios service
