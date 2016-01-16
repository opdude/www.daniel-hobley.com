---
title: Managing IPs with SaltStack
date: 2016-01-08 19:59:16
author: Daniel Hobley
tags: 
- SaltStack
- Tips
---

Managing things with SaltStack can be a real walk in the park but some things are not quite as obvious when you first start out. One thing I've been asked to explain is how I manage setting IPs for networking adapters on our servers. 

With out recent move to our new hosting center all of our servers are being re-built to using SaltStack and with this the opportunity came to design a system that could determine how all the networking, DHCP and DNS settings could be set in a single simple location.
<!-- more -->

First we start out with setting some basic info about our staging network into a jinja2 file that will be imported later.
``` jinja /srv/pillar/network/map.jinja2
{%  set production = {
        'address': '192.168.0.1',
        'gateway': '192.168.0.1',
        'netmask': '255.255.255.0',
        'ranges' : ['192.168.0.10', '192.168.0.200']
    }
%}

{%  set staging = {
        'address': '192.168.1.1',
        'gateway': '192.168.1.1',
        'netmask': '255.255.255.0',
        'ranges' : ['192.168.1.50', '192.168.1.100']
    }
%}
```

Next we add some servers adding the networking info to each of the servers so that we can get the general information directly from the minions pillars.

``` jinja /srv/pillar/network/map.jinja2
{%  set nics = {
    'dhcp-01': [
        {'id': 'eth0', 'ip': '192.168.0.2', 'env': production},
        {'id': 'eth1', 'ip': '192.168.1.2', 'env': staging},
    ],
    'dns-01': [
        {'id': 'eth0', 'ip': '192.168.0.4', 'env': production},
    ],
    'dns-02': [
        {'id': 'eth0', 'ip': '192.168.0.5', 'env': production},
    ],
    'staging-proxy-01': [
        {'id': 'eth0', 'ip': '192.168.0.7', 'env': production},
    ],
    'staging-proxy-02': [
        {'id': 'eth0', 'ip': '192.168.0.8', 'env': production},
    ]
%}
```

Now we have a basic structure that we can import and use to produce pillar information.

```jinja /srv/pillar/network/init.sls

# Set our basic domain and fqdn settings
{% set domain = "my.domain.com" %}
{% set fqdn = grains.id + "." + domain %}

{% from "network/map.jinja2" import nics with context %}
  {% set network_info = network_info %}

  {% if 'dns' in grains.id %}
  {% set dns = [
    "8.8.8.8",
    "4.4.4.4"
    ] %}
  {% else %}
  {% set dns = [
    network_info['dns-01'][0]['ip'],
    network_info['dns-02'][0]['ip'],
    ] %}
  {% endif %}
{% endif %}


# Setting this here means we can import it in another pillar file later on

{% set network =
      {
        "domain": domain,
        "fqdn": fqdn,
        "dns": dns,
        "nics": nics
      }
%}

# Set the networking data for this specific minion
network:
  {% if nics is defined and nics is not none %}
  nics:
  {% for nic in nics %}
    -
      id: {{ nic.id }}
      ip: {{ nic.ip }}
      netmask: {{ nic.env.netmask }}
      gateway: {{ nic.env.gateway }}
  {% endfor %}
  {% endif %}
  domain: {{ domain }}
  fqdn: {{ fqdn }}
  {% if dns is defined %}
  dns: {{ dns }}
  {% endif %}
 ```

 Now you have networking data that you can use to create states that will setup IP information on all of your minions. Cool Huh?

 You can take this further and also build out DHCP and DNS configurations using the same data which means that you have all of your IP data in one location!