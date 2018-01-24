# .vimrc
``` 
autocmd FileType yaml setlocal ai ts=2 sw=2 et
```
# Ad-hoc commands
`-m command` is default and can be omitted

Output on one line with `ansible ... -o`

`-m shell` when using `|` or `>`

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
Ansible looks for its configuration file in a number of places in order of precedence. The first configuration file found is used. All others are ignored.

1. ` $ANSIBLE_CONFIG` 
1. ` ./ansible.cfg`
1. `~./ansible.cfg`
1. `/etc/ansible/ansible.cfg` 

# Multi-line strings
## Preserve newline characters within the string
```
include_newlines: |
          ACME
          123 Main Street
          Atlanta, GA 30303
```

## Convert newline characters to spaces and remove leading white spaces
```
fold_newlines: >
          This is
          a very long,
          long, long, long
          sentence.
```

# Playbooks
## Syntax check
`ansible-playbook --syntax-check playbook.yml`

## Variables
If the same variable name is defined at mre than one level, the higher wins. Variables defined by the inventory are overridden by variables defined by the playbook, which are overridden by variables defined on the command line.

It is recommended practice to define inventory variables using `host_vars` and `group_vars` directories, and not to define them directly in the inventory file or files.

### Definition in playbook
```
- hosts: all
  vars:
    user: joe
    comment: "Joe"
```

### Definition in vars_files
```
- hosts: all
  vars_files:
    - vars/users.yml
```
`vars_files` wins over `vars`.
