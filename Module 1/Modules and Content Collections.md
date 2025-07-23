- In Ansible 2.9 and earlier, thousands of modules were provided
- Since Ansible 2.10, only essential modules are provided in the ansible-core package (67)
- Additional modules are provided through content collections
	- Ansible Galaxy (community)
	- Ansible Automation Platform (AAP)
	- Directly from tar archives
- ansible-navigator gets module from content collections using its container-based execution environment (so no need for Ansible control host)

Setting up execution environment:
- Login to registry
- `ansible-navigator images` to download the execution environment container image, "shows ee-supported-rhel8 image"
- Press Esc to exit image list

Ansible modules:
- Fully Qualified Collection Name (FQCN)
	- `ansible.builtin.service`
	- `ansible.posix.firewalld`
- **ansible-doc** provides documentation about all aspects of Ansible
- `ansible-doc -l` to list all available modules
- `ansible-doc [-t itemtype] item` to show usage information
- `ansible-navigator` can also provide help about item
	- `ansible-navigator doc -m stdout ansible.core.ping`
- `ansible-navigator --pp never` to not to check for newer container image


Specify collection path inside config `collections_path=./collections`

Installing collections:
- From Galaxy: `ansible-galaxy collection install my.collection -p collections`
- From tar ball: `ansible-galaxy collection install /my/collection.tat.gz -p collections`
- From a URL: `ansible-galaxy collection install https://my.example.local/my.collection.tar.gz -p collections`
- From Git also: `ansible-galaxy collection install git@git.example.local:user/mycollection.git -p collections`

#### requirements.yml

- A requirements.yml file can be provided in the current project directory to list all collections that are needed in the project
- It lists all required collections, and installs them using `ansible-galaxy collections install -r collections/requirements.yml -p collections`

An example for that:

```yaml
collections:
  - name: community.aws
  - name: ansible.posix
    version: 1.2.1
  - name: /tmp/collection.tar.gz
  - name: http://www.example.com/collection.tar.gz
  - version: main
```

#### Essential Modules

- `ansible.builtin.ping`: verifies host availability
- `ansible.builtin.service`: checks if a service is currently running
- `ansible.builtin.command`: runs any command but not through a shell (**not idempotent**)
	- `ansible all -m command -a "/sbin/reboot -t now"`
- `ansible.builtin.shell`: runs arbitrary commands through a shell
- `ansible.builtin.raw`: runs a command on a remote host without a need for Python
- `ansible.builtin.copy`: copies a file to the managed host