Ansible:

- A configuration (state) management software
- It uses modules in YAML files to _describe_ the current state of managed systems
- Using Ansible, the desired state is compared to the current state of managed systems
- If there is a state diffing, Ansible will update the current state to the desired state
- It is developed on top of Python, and manifest files written in YAML
- It is agentless, so it uses SSH to reach out managed systems

Community Ansible:

- Ansible core is open-source software [GitHub repo](https://github.com/ansible/ansible) but it provides only essential modules
- Additional modules can be found through [Ansible Galaxy](https://galaxy.ansible.com)
- Ansible AWX an open-source web-based Ansible.
	- Provides REST API and a task engine
- ansible-navigator a TUI Ansible

Red Hat Ansible:

ansible-core package is an independent package in RHEL 9, using it doesn't require Ansible Automation Platform (AAP).

AAP bundles different open source components:

- ansible-core
- Ansible Automation Controller, based on AWX
- Ansible Content Collection through Automation Hub, available on https://console.redhat.com
- Automation content navigator (ansible-navigator), introduced as alternative to ansible-playbook and older commands
- Automation Execution Environment, which contain runtime environment with specific modules and other content, used by ansible-navigator

Automation Execution Environments:

It is a container image that contains Ansible Core, Ansible Content Collections and all dependencies needed to run a playbook. Otherwise, you have to setup Ansible Control Node to contain all the software to manage your environment.

Execution Environment were introduced to offer a complete solution.

When using ansible-navigator, an execution environment is used to start the playbook.

ansible-builder tool can be used to build custom execution environments.

#### ansible-navigator

Default execution environment is provided when using ansible-navigator. Additional execution environment can be loaded from navigator.

Within an execution environment, specific content collections can be provided.