- Ad-hoc commands can be used to run one or a few tasks
- Convenient to test, or when a complete managed infrastructure hasn't been set up yet
- Playbook are used run multiple tasks 
- In playbooks, one or multiple plays are started
	- Each play runs one or more tasks
	- In these tasks, different modules are used to perform the actual work
- Written in YAML

Example Playbook

```yaml
---
- name: deploy vsftpd
  hosts: ansible1
  tasks:
	- name: install vsftpd
	  yum: name=vsftpd
	- name: enable vsftpd
	  service: name=vsftpd enabled=true
	- name: create readme file
	  copy:
		content: "VSFTPD installed"
		dest: /var/ftp/pub/README
```

- Running a playbook with `ansible-playbook vsftpd.yml`
- With ansible-navigator: `ansible-navigator run -m stdout vsftpd.yml`
- Verify syntax: `ansible-playbook --syntax-check vsftpd.yml`

##### Plays

- Playbook can contain multiple plays
- A play is a series of tasks that are executed against selected hosts in inventory, using specific credentials
- Multiple plays allows running tasks on different hosts with different credential from the same playbook
- You can set escalation parameter within a play definition:
	- `remote_user`, `become`, `become_method`, and `become_user`
