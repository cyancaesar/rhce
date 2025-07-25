- Roles are about reusable code
- Provides a standardized solutions
- Community roles are found on Ansible Galaxy
- Can also be found on Ansible Content Collection from Red Hat automation hub
- Roles are included in tasks with `roles:` section in the playbook play header

```bash
ansible-galaxy role install <ROLE> -p <PATH>
```

`roles_path` setting is to specify roles path installation, same as collections

Roles precedence:
- roles directory on the current project directory
- `~/.ansible/roles`
- `/etc/ansible/roles`
- `/usr/share/ansible/roles`

Searching and managing for roles:
- `ansible-galaxy [role] search 'docker' --author geerlingguy --platforms EL`
- `ansible-galaxy [role] info geerlingguy.docker`
- `ansible-galaxy [role list]`
- `ansible-galaxy [role] remove geerlingguy.docker`

`roles/requirements.yml` to specify roles that are needed in the project:

Via git:
```
https://github.com/user/roles
scm: git
```

Via files:
```
file:///tmp/myrole.tar
```

Via web:
```
https://example.com/myrole.tar
```

```yml
- src: https://github.com/mygit/myrole
  scm: git
  version: "1.0.0"
- src: file:///tmp/myrole.tar
  name: myrole
- src: https://example.com/mywebrole.tar
  name: mywebrole
```

#### Using Roles

Roles can be used in playbooks by:
- Using `roles:` section in the play header
- Using `import_role` or `include_role` module in a task

Roles listed in the `roles:` section are executed first before the tasks in the play.

`pre_tasks` to force the tasks to run first before the roles execution, while `post_tasks` are tasks that are run after roles and tasks


> [!tip] Tip
> If a role should be executed after some tasks, use `import_role` inside the tasks

`include_roles` dynamically load roles, while `import_role` loads the role are the process time of the playbook. Use `import_role` because all of its variables and handlers are available.

Variables can be set as a role parameter when calling the role:

```yml
- roles:
    - role: myrole
      param: myvalue
```

Variable inside role are either from `default` directory (intended to overwritten) or from `vars` directory (internal).

#### Custom Roles

- Structure the role directory and make sure it is inside the `roles_path`
- `ansible-galaxy role init myrole` to create a default directory structure
- Complete `main.yml` files for role content

#### RHEL System Roles

- RHEL system roles are provided as an interface to manage specific parameters between different RHEL versions
- `dnf install rhel-system-roles`
	- Install it for a free sample playbooks in `/usr/share/doc/rhel-system-roles/`

```
- Create a requirement file to install `gearlingguy.ngix` and `gearlingguy.docker` roles

- Configure your role path to install newly installed roles in the current working directory

- Use RHEL system role to config timesync against `pool.ntp.org` time servers

- Create a custom role that generates an `/etc/issue` file that contains `Today is <DATE>, welcome to <SYSTEM>`
```