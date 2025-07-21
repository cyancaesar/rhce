- Inventory in a minimal form, is a set of host names and/or IP addresses
- Hosts can be grouped
- A single host can be a member of multiple groups
- Variables is can be used to set the inventory file _deprecated_
- Ranges are also supported
- `/etc/ansible/hosts` default inventory
- `ansible.cfg` can be used to specify inventory file
- Or by using `-i inventory`

Static inventory:

```
[webservers]
web[1:9].example.com
web12.example.com

[fileservers]
file1.example.com
file2.example.com

[server:children]
webservers
fileservers
```

Host groups can be:
- Functional
	- web
	- lamp
- Regional
	- europe
	- middle_east
- Staging
	- test
	- development
	- production

To test inventory:

```bash
ansible -i inventory all --list-hosts
ansible -i inventory file --list-hosts
ansible-navigator inventory -i inventory -m stdout --list
```