Nautobot
=====

Ansible role to install https://nautobot.readthedocs.io/en/latest/[Nautobot CMDB]

###NOTE!
This version is on very early development state as of 28 april 2021.
Not tested very well, some procedures not automated yet!

Please be aware that NAPALM credentials are not shared between Nautobot and evenutally Nautobot plugin Nonir (golder-config). So when using that plugin you need to set the credentials in the nautobot-service file also

##Manual tasks after run:
* nautobot-server createsuperuser
see: https://nautobot.readthedocs.io/en/latest/installation/nautobot/#prepare-the-database

# dependencies:
geerlingguy.postgres
geerlingguy.redis

Install roles:
```
ansible-galaxy install geerlingguy.postgres
ansible-galaxy install geerlingguy.redis
ansible-galaxy install bsmeding.nautobot
```

## Install CMDB on localhost
Ensure ansible is installed (`pip3 install ansible`)

create install playbook (Example:)
```
---
- name: Install CMDB Nautobot
  hosts: localhost
  gather_facts: true
  become: yes
  vars:
    nautobot_use_nginx: True
    nautobot_nginx_allow_local_modifications: False
    nautobot_nginx_use_ssl: True
    nautobot_python_deps_updated: True
    nautobot_config:
      ALLOWED_HOSTS:
        - "*"   # Do not use in production!
        - localhost
        - 127.0.0.1
      NAPALM_USERNAME: 'cisco'
      NAPALM_PASSWORD: 'cisco'
      NAPALM_ARGS:
        secret: 'enable'
      TIME_ZONE: "CET"
      BANNER_TOP: "Nautobot DMDB"
      BANNER_BOTTOM: "Nautobot CMDB"
      BANNER_LOGIN: "Nautobot CMDB"
    nautobot_plugins:
      - plugin_name: nautobot_device_onboarding
        pip_module: nautobot-device-onboarding
        plugin_config:                # Dict with config times
          create_platform_if_missing: True
          default_device_role: "network_onboarding"
  tasks:

    - include_role:
        name: bsmeding.nautobot

```


### proxy
to use a proxy, pass the settings in environment variables
`export http_proxy="http://192.168.10.1:80"`
`export https_proxy="http://192.168.10.1:80"`

### Debug
Login as root
switch to nautobot user `su nautobot`
Go to home dir: `cd ~/`


### Notes
Please not that the SECRET_KEY is saved to the Nautobot directory, please let this file there otherwise the next run a new key will be generated and than the database connecation can't be made.
And save this key to external password vault for reverence, loosing this key is loosing you're data!
