Managing networking is convenience through using RedHat system roles: `redhat.rhel_system_roles.network`.

Configuring the role by setting `network_provider` and `network_connections` variables.
- `network_provider` must be set to `nm`.

Network related modules and facts:
`ansible.posix.firewalld`
`hostname`
`ansible_facts['interfaces']`


