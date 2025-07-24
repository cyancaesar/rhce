- A variable is a label that is assigned to a specific value to refer to that value throughout the playbook
- A fact is a special type of variable, that refers to a current state of an Ansible-managed system
- Like define a variable `web_service` for Ubuntu and Red Hat systems
	- And refer to this variable instead of the actual service name
	- Ubuntu uses `apache2` package and Red Hat uses `httpd` package

Defining a variable can be
- On a playbook
- Includes files of variables
- On a command line arguments
- The output of a command or task can be used in a variable using `register`
- `vars_prompt` for interactive
- ansible-vault is used to encrypt sensitive values
- Facts are discovered host properties stored as variables
- Host variables are host properties that don't have to be discovered
- System variables are a part of the Ansible system and cannot be changed

Variable precedence:
- Global scope: from inventory or the command line
- Play scope: applied when it is set from a play
- Host scope: applied when set in inventory or using host variable inclusion file

```yaml
- hosts: all
  vars:
	web_package: httpd
```

Or with a variables file:

```yaml
- hosts: all
  vars_files:
	- vars/envs.yml
```

Referring to a variable as `{{ web_package }}`. But exception when using *when* and jinja template:

```yaml
when: "'not found' in command_result.err"
```

```jinja2
{% if ansible_facts['devices']['sdb'] is defined %}
	Secondary disk size: {{ ansible_facts['devices']['sdb']['size'] }}
```

If a variable is the first element, using quotes is mandatory: `"{{ web_package }}"`


Host variable:
- Variables that are specific for a host only
- Defined in a YAML file that has the name of the inventory hostname and are stored in the `host_vars` directory in the current project directory
- Applying variables to host groups, a file with the inventory name of the host group should be defined in the `group_vars` directory in the current project directory
- Host variables can be set in the inventory, but it is now deprecated

System variable:
- Built in variables and cannot be used for anything else
- `hostvars` a dictionary of all variables that apply to a specific host
- `inventory_hostname`: inventory name of the current host
- `inventory_hostname_short`: short host inventory name
- `groups`: all hosts in inventory, and groups these hosts belong to
- `group_name`: list of groups the current host is a part of
- `ansible_check_mode`: boolean that indicates if play is in check mode
- `ansible_play_batch`: active hosts in the current play
- `ansible_play_hosts`: same as above
- `ansible_version`: current Ansible version

Register the task result with `register` keyword.

Also Vaults are used to manage sensitive values.

Managing a vault:

```bash
mkdir -p host_vars/ansible1
echo "user: deployer" > host_vars/ansible1/vars
cd host_vars/ansible1
ansible-vault create vault # enter the vault password
ansible-playbook --ask-vault-pass create-user.yml

echo "secretpassword" > vault-pass
ansible-playbook --vault-password-file vault-pass create-user.yml
```