Modules  to work mounts with  storages:
- `ansible.posix.mount`: mount existing filesystems
- `community.general.parted`: manage partitions
- `community.general.lvg`: manage volume groups
- `community.general.lvol`: manage logical volumes
- `community.general.filesystem`: create filesystems on the new devices
- `redhat.rhel_system_roles.storage`: a RedHat system role that works on RHEL storages

The `redhat.rhel_system_roles.storage` is inconvenience role, it supports only the following:
- unpartitioned devices, which it only creates a filesystem on a whole disk device
- LVM on a whole disk device

Creating LVM using RHEL system role, define `storage_pools` variable with its variables:
- `name`: volume group name
- `disks`: disk to use
- `volumes`: LVM logical volumes and their properties

### Advanced Playbook Strategy

It is recommended when developing advanced playbook solution to follow a phased approach:
- Setting up a generic framework that shows how you are going to take care of any conditional task execution. Put as much as you can on the `debug` module and focus on the structure
- Work out the conditionals and facts that may be used to check, using also the `debug` module
- Work out the module specifics and produce a working solution

Advanced playbook using storage example requirements:

- Tasks in this playbook should only be executed on hosts where a second hard disk is installed
- If no second hard  disk exists, the playbook should print "second disk not present" and stop executing tasks on that host
- Configure the device with one partition including all available disk space
- Create an LVM volume group with the name `vgfiles`
- If the volume group is bigger than `5GB`, create an LVM logical volume with the name `lvfiles` and a size of `5GB`. Note that you must check the LVM Volume Group size an not the `/dev/sdb` size, because in theory you could have multiple block devices in a volume group
- If the volume group is equal to or smaller than `5GB`, create an LVM logical volume with the name `lvfiles` and a size of `3GB`
- Format the volume with the XFS filesystem
- Mount it on the `/files` directory