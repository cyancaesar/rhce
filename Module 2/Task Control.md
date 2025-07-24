Conditional that are available in Ansible:
- `loop`: iterate over a list of items
- `when`: perform a conditional task
- `handlers`: perform a task if and only if another task triggered it

#### Loops

```yaml
- name: Start services
  service:
    name: "{{ item }}"
    state: started
  loop:
    - vsftpd
    - httpd

# With a variable
  loop: "{{ services }}"
```

#### When

A condition that can run a task only if its evaluation are true.

Example conditions:

```py
ansible_machine == 'x86_64'
ansible_distribution_version == '8'
ansible_memfree_mb == 1024
ansible_memfree_mb < 256
ansible_memfree_mb > 256
ansible_memfree_mb <= 256
ansible_memfree_mb != 512
my_var is defined
my_var is not defined
my_var
ansible_distribution in supported_distros
```

Filters can be used to enforce type casting: 
- `when: vgsize | int > 5`
- `when: runme | bool`

You can also make the when statement to have a multiple conditions using `and` or `or`

```
when: ansible_distribution === 'RedHat' or \
ansible_distribution === 'CentOS'
```

You can also use a list of when to make a complex when, and all the list items must be true.

`register` is useful to register a variable of a task and checking it with a `when`

#### Handlers

- It only triggers if a task has changed something
- `notify` statement is used from the main task
- Handlers _typically_ are used to restart services and reboot hosts
- Handlers are executed after running all tasks in a play
- `meta: flush_handlers` to run handlers now
- `force_handlers: true` to forces run handlers if a task fails

```yaml
- name: Copy index.html
  copy:
    src: /tmp/index.html
    dest: /var/www/index.html
  notify:
    - restart_web

handlers:
  - name: restart_web
    service:
      name: httpd
      state: restarted
```

Ansible Meta `ansible.builtin.meta` is a module that can change the internal order execution of Ansible

---

#### Block

- A logical group of tasks that can be used to control how tasks are executed.
- One block can be enabled with a single `when`
- It can be used for error handling
	- A `block` for main task
	- A `rescue` to define tasks that run if tasks defined in the block fail
	- `always` to define tasks that always run

Failure handling on plays:
- Ansible checks the exit status of a task to determine if it failed
- When tasks fails, it aborts the play for that specific host
- `ignore_errors` to ignore the failures
- `failed_when` to dictate Ansible to look for in a command output to recognize failure

```yaml
- name: Run a script
	command: echo hello world
	ignore_errors: yes
	register: command_result
	failed_when: "'world' is not in command_result.stdout"
```

`fail` module:
- `failed_when` keyword to specify error condition
- `fail` module is used to print a message of why the task failed
- When using `fail` module, set `ignore_errors` to yes

Managing changed status with `changed_when`

 #### Inclusion
- Include is a dynamic process
- Import is a static process `import_playbook`
- Task can also be included `import_tasks` to statically import a task file in the playbook
- `include_tasks` to dynamically include a task file