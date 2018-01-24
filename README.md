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

# Playbooks
## Syntax check
`ansible-playbook --syntax-check playbook.yml`


