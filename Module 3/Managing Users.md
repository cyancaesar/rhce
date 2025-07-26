Users related modules:
- `user`
- `group`
- `known_hosts`
- `authorized_key`
- `lineinfile` generic module, but useful when configuring sudo access

`append: true` option is useful when adding a user to secondary groups

`lineinfile` and `template` can be used to manage `sudoers.d`.

`validate: /usr/sbin/visudo -cf %s` to validate sudoers modification