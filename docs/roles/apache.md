# Ansible Role - Apache

Manages the Apache2 package and associated tasks.

### Variables

`html_content` - Defined in `<inventory>/host_vars/ops[1,2]`. Dictates the content of the returned HTML web page from the webserver pool.

### Tasks

`Install Apache2` - Install the Apache2 package
`Enable/start Apache service` - Start the Apache2 service
`Define custom log format` - Creates a custom log format to register the forwarded IP address in access logs
`Override default log format` - Enables custom access log
`Overwrite default index.html` - Assigns default `index.html` the content of `html_content` variable

### Templates

None

### Handlers

`restart apache2` - Restart Apache2 service
