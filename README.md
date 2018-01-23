# .vimrc
``` 
autocmd FileType yaml setlocal ai ts=2 sw=2 et
```

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
Newline characters within the string are preserved:
```
include_newlines: |
          ACME
          123 Main Street
          Atlanta, GA 30303
```



one line:
`ansible ... -o`

`-m command` is default and can be omitted

`ansible-playbook --syntax-check playbook.yml`

`-m shell` when using `|` or `>`
