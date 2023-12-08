---
title: Ansible Jinja2 Template Layouts
date: 2023-12-07
tags:
  - Ansible
  - Jinja2
  - systemd
description: Applying the systemd drop-in configuration method to Jinja2 templates in Ansible.
permalink: posts/{{ title | slug }}/index.html
---
## Problem
At work we have circled the wagons on Ansible for network automation.  When it
comes to automation the team that I am on is relatively inexperienced. Having a system
that generates relatively familiar information is a definately in the plus
column.  Where it gets tricky is we are a service provider and our configs are
long... I have been formulating a solution that should bring us home.


## Solution
The configuration are broken out at first by "top level" configuration
stanzas(groups, system, protocols, etc).  For every one of the top level
configuration files there is a ".d" directory created for smaller configuration
pieces.

The result looks something similar to this:
```
.
└── templates
    ├── groups.d
    │   ├── groups-header.j2
    │   └── tacplus-servers.j2
    ├── groups.j2
    ├── system.d
    │   ├── system-header.j2
    │   └── tacplus-servers.j2
    └── system.j2

3 directories, 6 files
```

The `tacplus-server.j2` template extends the `groups-header.j2` file.  This
contains the outside block `groups {}` block in which we will be inserting
configuration inside of for each group configuration that we generate.

```
# templates/groups.d/groups-header.j2
{% raw %}
groups {
    {% block group_content %}{% endblock %}
}
{% endraw %}
```
As you can see I have defined a Jinja content block to to place the user
defined groups configuration in.

Then in the tacplus configuration below we are extending group config and
defining the content block.

```
# templates/groups.d/tacplus-server.j2
{% raw %}
{% extends 'groups-header.j2' %}
{% block group_content %}
    replace:
    tacplus-servers {
        system {
            tacplus-server {
                <*> {
                    source-address {{ mgmt_ip }}
                    secret {{ vaulted_tacacs_secret }}
                    port 49
                }
            }
        }
    }
{% endblock %}
{% endraw %}
```

In the main `groups.j2` template all we have to do is include any number of
groups that we are defining.

```
# templates/groups.j2
{% raw %}
{% include 'groups.d/tacplus-servers.j2' %}
{% endraw %}
```

The drawback to this approach is when you have more than one group defined. You
will end up rendering configuration with multiple `group {}` blocks with one
user defined groups configuration in it.

It will look funky, but Junos can figure it out.

The benfit from this methodology is we can define configuration in a service
oriented fashion and do targeted changes to that service.

Lets fill out the rest of the templates so you can see what I mean.  We will
take the same approach on the system configurations.
```
# templates/system.d/system-header.j2
{% raw %}
{% block system_content %}{% endblock %}
{% endraw %}
}
```

```
# templates/system.d/tacplus-servers.j2
{% raw %}
{% block system_content %}
    tacplus-server {
{% for server in tacplus_servers %}
        {{ server }};
{% endfor %}
    }
{% endblock %}
{% endraw %}
```

```
# templates/system.j2
{% raw %}
{% include 'system.d/tacplus-servers.j2' %}
{% endraw %}
```

So now lets define just the tacacs service in a file.
```
# /templates/service-tacacs.j2
{% raw %}
{% include 'groups.d/tacplus-servers.j2' %}
{% include 'system.d/tacplus-servers.j2 %}
{% endraw %}
```

I know we can send the entire configuration to the router and Junos will decide
what has changed and what hasn't, but we are still in a brown field environment
and I would like to stay very tactical with our configuration changes.  If we
are not careful and we have a "cowboy config" out in the network we could end
up isolting a router or not being able to manage it.


