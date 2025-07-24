- Ansible Facts are special variables that are automatically discovered by Ansible on managed hosts
- Fact contain information about hosts that can be used in conditionals
- Using facts allows you to branch the actions flow
- All playbooks perform fact gathering by default, can be turn off with `gather_facts: no` in the play header
- Can be enabled in the task with `setup` module
- To show facts use the debug module to print the value of `ansible_facts`
- In Ansible 2.5 and later, all facts are stored in one dictionary called `ansible_facts`

```py
# Since Ansible 2.4 and earlier
ansible_date_time.date

# Ansible 2.5 and later
ansible_facts.date_time.date

# Preferred notation
ansible_facts['date_time']['date']
```

Dictionaries can be written in two ways:

```yaml
users:
  cyan:
    username: cyan
    shell: /bin/zsh
  admin:
    username: admin
    shell: /bin/bash
```

Or as:

```yaml
users:
  cyan: { username: 'cyan', shell: '/bin/zsh' }
  admin: { username: 'admin', shell: '/bin/bash' }
```

#### Custom Facts

- Host variables exist on the control machine and not in the managed machine
- If another control machine takes control, it needs those host variables
- Custom facts allow administrators to _dynamically_ generate variables which are on the host stored as facts. And it is stored on `/etc/ansible/facts.d` directory on the managed hosts
	- Must ends with `.fact` and must have `[label]` to help identify the variables
- Accessing the custom facts on `ansible_facts.ansible.local`

A custom fact: 

```ini
[software]
package = httpd
service = httpd
state = enabled
```

Back to the variable scope, we could set a variable to the entire playbook by:
- Host or host group
- Inventory
- vars plugin
- Using modules like `set_fact` and `include_vars`

