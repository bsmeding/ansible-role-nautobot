Nautobot
=====

Ansible role to install https://nautobot.readthedocs.io/en/latest/[Nautobot CMDB]

###NOTE!
This version is on very early development state as of 21 april 2021.
Not tested very well, some procedures not automated yet!

##Manual tasks after run:
* nautobot-server migrate  
* nautobot-server createsuperuser
* nautobot-server collectstatic
see: https://nautobot.readthedocs.io/en/latest/installation/nautobot/#prepare-the-database


# dependencies:
geerlingguy.postgres
geerlingguy.redis

Install:
```
ansible-galaxy install geerlingguy.postgres
ansible-galaxy install geerlingguy.redis
ansible-galaxy install bsmeding.nautobot
```

# How to use


### Debug
Login as root
switch to nautobot user `su nautobot`
Go to home dir: `cd ~/`


### Notes
Please not that the SECRET_KEY is saved to the Nautobot directory, please let this file there otherwise the next run a new key will be generated and than the database connecation can't be made.
And save this key to external password vault for reverence, loosing this key is loosing you're data!
