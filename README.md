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
1. ` $ANSIBLE_CONFIG` 
1. ` ./ansible.cfg`
1. `~./ansible.cfg`
1. `/etc/ansible/ansible.cfg` 


one line:
`ansible ... -o`

`-m command` is default and can be omitted

`ansible-playbook --syntax-check playbook.yml`

`-m shell` when using `|` or `>`
