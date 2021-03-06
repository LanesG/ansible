# .vimrc
``` 
autocmd FileType yaml setlocal ai ts=2 sw=2 et
```
# Ad-hoc commands
`-m command` is default and can be omitted

Output on one line with `ansible ... -o`

`-m shell` when using `|` or `>`

# Configuration file
## Get config file location
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

# Playbook syntax check
`ansible-playbook --syntax-check playbook.yml`

# Variables
If the same variable name is defined at more than one level, the higher wins. Variables defined by the inventory are overridden by variables defined by the playbook, which are overridden by variables defined on the command line.

It is recommended practice to define inventory variables using `host_vars` and `group_vars` directories, and not to define them directly in the inventory file or files. Host variables take precedence over group variables, but variables defined by a playbook take precedence over both.

The `include_vars` module is one more way to set variables in a playbook from an external file. It overrides any values set through the methods above.

When a variable is used as the first element to start a value, quotes are mandatory:
```
  with_items:
    - "{{ foo }}"
```

## Definition in playbook
```
- hosts: all
  vars:
    user: joe
    comment: "Joe"
```

## Definition in vars_files
```
- hosts: all
  vars_files:
    - vars/users.yml
```
`vars_files` wins over `vars`.

## Registered variables
```
    command: ps
    register: ps_result
  
  - debug:
    msg: "{{ ps_result.stdout }}"
```
or
```
  - debug:
    var: ps_result
    verbosity: 2
```

## Magic variables
`hostvars` lets you ask about the variables of another host, including facts that have been gathered about that host. If, at this point, you haven’t talked to that host yet in any play in the playbook or set of playbooks, you can still get the variables, but you will not be able to see the facts.

`group_names` is a list (array) of all the groups the current host is in. This can be used in templates using Jinja2 syntax to make template source files that vary based on the group membership (or role) of the host.

`groups` is a list of all the groups (and hosts) in the inventory. This can be used to enumerate all hosts within a group.

`inventory_hostname` is the name of the hostname as configured in Ansible’s inventory host file. This can be useful for when you don’t want to rely on the discovered hostname `ansible_hostname` or for other mysterious reasons. If you have a long FQDN, `inventory_hostname_short` also contains the part up to the first period, without the rest of the domain.

`inventory_file` is the pathname and the filename pointing to the Ansible’s inventory host file.

# Loops
```
  service:
    name: "{{ item }}"
    state: started
  with_items:
    - postfix
    - dovecot
```

```
  mysql_user:
    name: "{{ item[0] }}"
    priv: "{{ item[1] }}.*:ALL"
  ...
  with_nested:
    - [ 'joe', 'jane' ]
    - [ 'clientdb', 'employeedb', 'providerdb' ]
```

|Loop Keyword|Description|
| --- | --- |
|`with_file`|Takes a list of control node file names. `item` is set to the content of each file in sequence.|
|`with_fileglob`|Takes a file name globbing pattern. `item` is set to the content of each file in a directory on the control node that matches that pattern, in sequence, non-recursively.|
|`with_sequence`|Generates a sequence of items in increasing numerical order. Can take `start` and `end` arguments which have a decimal, octal, or hexadecimal integer value.|
|`with_random_choice`|Takes a list. `item` is set to one of the list items at random.|
