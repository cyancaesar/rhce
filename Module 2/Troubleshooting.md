Logging is by default to the standard out, `log_path:` to specify log directory.

`debug` module is useful to view the variable:
- `verbosity` argument to specify when this runs.
- `verbosity: 2` will show the debug output when `-vv` is passed

Check mode:
- `--check` option is passed while running a playbook to perform the execution without actually applying the changes. Its like a dry run
- `check_mode: yes` set it within a task to always run that task on a check mode
- `--diff` running diff on template files on a managed hosts

Modules for checking and testing:
- `uri`: checks content from a URL
- `script`: runs a script from the control node on the managed hosts
- `stat`: return the file stats
- `assert`: a module that fails when a specific condition is not met

`stat` returns useful items:
- `atime`
- `isdir`
- `exists`
- `size`

Authentication issues:
- Confirm `remote_user` exists on the managed machine
- Confirm host key setup
- Verify `become`, `become_user`
- Check Linux sudo if configured correctly
	- Easy to verify by using `command` module and ls the root directory
- `ping` module to verify connectivity

`ansible_host` can be set on the inventory to specify host IP address:

```ini
ansible1 ansible_host=46.182.104.101
```

