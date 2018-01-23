# Configuration file
## Get used config file
```
ansible --version
...
  config file = ...
```

or

```
ansible dev -i inventory --list-hosts -v
Using ... as config file
...
```
## Configuration file precedence
1. ` $ANSIBLE_CONFIG` 
1. ` ./ansible.cfg`
1. `~./ansible.cfg`
1. `/etc/ansible/ansible.cfg` 
