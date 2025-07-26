Before managing software, these tasks need to be done:
- Systems need to be subscribed
- Repositories and software channels need to be configured

Modules to manage software:
- `dnf`
- `yum`
- `apt`
- `package`
- `ansible.windows.win_package`

Installation of a package group with `dnf`:

```yml
- name: Install Virtualization Host group
  dnf:
    name: '@Virtualization Host'
    state: present
```


Package facts are not gather by `setup` module, `package_facts` module can be used to gather facts about modules.

Accessing this facts on `ansible_facts['packages']`

#### Repository

`yum_repository` module is used to access the repositories. The module creates a repository file in `/etc/yum.repos.d/` directory.

If `gpgcheck: yes` argument is passed to the module, `rpm_key` module must be used to install the GPG key.

`redhat_subscription` module enables you to perform subscription and registration in one task.

`rhsm_repository` module is used to enable subscription manager repositories.