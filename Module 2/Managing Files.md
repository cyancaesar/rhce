Modules for managing files:
- `lineinfile`: check a line is in a file
- `blockinfile`: modify multi-line blocks of text in files
- `file`: sets attributes to files, and can also create and remove files, symbolic links and more
- `stat`: request file statistics

Modules for copying stuff:
- `copy`: copies a file from a local machine to  a managed-machine
- `fetch`: the reverse of copy
- `ansible.posix.synchronize`: synchronizes files rsync style "only works if rsync exists on the managed host"
- `ansible.posix.patch`: applies patches to files

Jinja2 templates:
- `lineinfile` and `blockinfile` are used to apply simple modifications to files
- `template` renders the Jinja2 template to a final configuration file

To prevent administrators from overwriting files that are managed by Ansible, set `ansible_managed` string:
- In ansible.cfg: `ansible_managed = # Ansible managed`
- On top of a Jinja2 template, include: `# {{ ansible_managed }}`

```jinja2
{% for user in users %}
	{{ user }}
{% endfor %}

{% for host in groups['webservers'] %}
	{{ host }}
{% endfor %}
```

SELinux modules:
- `file`: can work with SELinux context
- `community.general.sefcontext`: manages SELinux file context in the SELinux Policy
- `command`: is required to run `restorecon` after `sefcontext`
- `file` DO NOT USE. Because it works lie `chcon`