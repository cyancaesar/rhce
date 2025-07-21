- `/etc/ansible/ansible.cfg` if exists, then it will be used
- If it exists on the current working directory, it will be used first
- Different connection can be specified

Settings in ansible.cfg are organized in two (or more) sections:
- `[defaults]` sets default settings
- `[privilege_escalation]` specifies how Ansible runs commands on managed hosts

Settings that are used:
- `inventory` path of inventory file
- `remote_user` the name of the user that logs in the remote hosts
- `ask_pass` whether or not to prompt for a password
- `become` if you want to automatically switch to the `become_user`
- `become_method` sets how to become the other user
- `become_user` specify the target remote user
- `become_ask_pass` if a password should be asked for when escalating privilege

Default connection protocol is SSH, other connection methods are available like `winrm` to manage Windows instances, `ansible_connection: winrm`, `ansible_port: 5986`

Escalating privileges in Ansible is, by default, using `sudo`. To setup password-less sudo, create a drop-in file in `/etc/sudoers.d/` with the content:

```bash
ansible ALL=(ALL) NOPASSWD: ALL
```

To securely escalate privileges using passwords, add the following to `/etc/sudoers`:

```bash
Defaults timestamp_type=global,timestamp_timeout=60
```

