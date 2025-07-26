Modules to manage processes:
- `service`: generic interface to used to manage the state of services
- `systemd`: manage the state of services in systemd
- `command` is used when no modules to manage services
- `reboot`: reboot a managed host

Modules to scheduling processes:
- `ansible.posix.at`
- `cron`