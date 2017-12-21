# Ansible Role - HAProxy

Manages the HAProxy load balancer

### Variables

* `haproxy` - Defined in `<inventory>/group_vars/balancers` - Configuration values related to the `haproxy.cfg` template

### Tasks

* `Add HAProxy 1.8 PPA` - Adds the HAProxy PPA for an up to date version of the package
* `Install HAProxy` - Installs HAProxy package
* `Configure HAProxy` - Configures `haproxy.cfg` template

### Templates

* `haproxy.cfg.j2` - Manages the master `haproxy.cfg` configuration

### Handlers

* `restart haproxy` - Restart HAProxy service
