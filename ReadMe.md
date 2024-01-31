# Admin Users

This is a role to manage administrators on servers.

## Supported OS

- Almalinux 8
- Almalinux 9

## Requirements

Variables:

```yaml
is_support_team: The name of the team that maintains the server
```

## Dependencies

None

## License

MIT / BSD

## Role Variables

```yaml
# Path to sudo.d
admins_sudo_path: /etc/sudoers.d

# Relevant path to search ssh keys
admins_ssh_dir: admins_keys/

# Admin group
admins_group: admins

# Base admin list
admins: []

# Extra admin list
admins_extra: []
```

[Search path](https://docs.ansible.com/ansible/latest/user_guide/playbook_pathing.html) provides information about searching for ssh keys.

### Admins variable structure

```yaml
admins:
  - name: Required. Name of the user.
    state: Optional. Default value is "present". For deletion is "absent".
    append: Optional. Default value is "False". Add Users to Other group.
    shell: Optional. Default omitted. User shell.
    groups: Optional. Default omitted. Additional groups list, must exist on hosts.
    remove: Optional. Default value is "True". Delete user home directory or not.
```

## Features

- No additional groups will be created, only the one specified in **admins_group** variable will be created
- User authentication using ssh keys only, password authentication disabled
- A home directory for users is always created
- Users are allowed to use a single ssh key file
- A user's SSH file can consist of multiple keys
- SSH key searches path add user name / User name adds to SSH key search's path
- The SSH key is always up to date
- The following comment is set for ssh keys: Ansible managed {{ user.name }}@{{ admins_group }}

## Example Variables

```yaml
---
admins:
  - name: baba.zina
    shell: /bin/zsh
  - name: foo.bo
    state: absent
  - name: anton.gatling
  - name: balkon.volkov
    append: true
    groups:
      - docker
admins_extra:
  - name: denis.user
```

## Example Playbook

```yaml
- name: Play - Configure Admin Users
  hosts: all
  roles:
    - admins
```
