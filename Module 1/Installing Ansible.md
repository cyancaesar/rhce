Install 3 VM's using RHEL
- `control.example.local` (Server with GUI)
- `ansible1.example.local` (Minimal install)
- `ansible2.example.local` (Minimal install)

The control node is the node that has Ansible installed on. Also configure a static IP and `/etc/hosts` file.

To use Ansible, the following requirements are needed:
- Host name resolution to address managed hosts by name
- Python on all nodes
- SSH running on the managed servers
- Dedicated user account with sudo privileges
- Optional: SSH keys
- An inventory to identify managed nodes on the control node
- Optional: `ansible.cfg` to specify default parameters

Installing Ansible (run with sudo)

```bash
# Verify the Red Hat repository exists
subscription-manager repos --list
# Enable repositories
subscription-manager repos --enable=rhel-9-for-x86_64-appstream-rpms
subscription-manager repos --enable=ansible-automation-platform-2.2-for-rhel-9-x86_64-rpms

# Install Ansible
dnf install ansible-core
dnf install ansible-navigator

# Verify
ansible --version
```

---

#### Setup Managed Nodes Using Ansible

```bash
cat >> inventory << EOF
ansible1
ansible2
EOF

ansible -i inventory all -u student -k -b -K -m user -a "name=ansible"
```

- `-i` specify the inventory file, with `all` parameter for all hosts
- `-u` the user of the managed server
- `-k` prompt the managed user password (no ssh)
- `-b` become
- `-K` become password
- `-m` specify Ansible module, user is an Ansible module
- `-a` module parameters, `name=ansible`

This will fail due to no host key checking exists, just ssh into the server and save the key and exit.

The previous Ansible creates only the user `ansible` but no password presents, to set a password:

```bash
ansible -i inventory all -u student -k -b -K -m shell -a "echo password | passwd --stdin ansible"
```

Generate an ssh key on the control node and copy it to the managed node to avoid entering the password every time you run Ansible:

```bash
ssh-keygen
for i in ansible1 ansible2; do ssh-copy-id $i; done
```

Make `ansible` in the sudoers file:

```bash
ansible -i inventory all -u student -b -K -m shell -a 'echo "ansible ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/ansible'
```

Run and test:

```bash
ansible -i inventory all -u ansible -b -m command -a "ls -l /root"
```

