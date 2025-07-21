Setting up ansible-navigator:

```bash
sudo dnf install ansible-navigator
ansible-navigator --version
podman login registry.redhat.io
podman pull registry.redhat.io/ansible-automation-platform-22/ee-supported-rhel8:latest
ansible-navigator images
ansible-navigator inventory -i inventory file --list
```

ansible-navigator generic settings can be found on `~/.ansible-navigator.yml`

Useful ansible-navigator settings:
- `pull.policy: missing` contact the container registry if no container image has been pulled
- `playbook-artifact.enable: false` required when a playbook prompts for a password

 