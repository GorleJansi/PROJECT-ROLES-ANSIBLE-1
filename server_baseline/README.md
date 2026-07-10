# Server Baseline Ansible Role

This project contains an Ansible role named `server_baseline`.

The purpose of this role is to configure a fresh Linux server with basic settings that are commonly used in daily DevOps work.

## What This Role Does

The `server_baseline` role performs these actions:

- Installs common packages such as `git`, `curl`, `vim`, `htop`, and `net-tools`
- Creates Linux users from variables
- Adds sudo access for the baseline users using a template
- Updates SSH security settings
- Restarts SSH only when SSH configuration changes
- Creates a login message using `/etc/motd`

## Role Structure

```text
server_baseline/
  tasks/main.yml
  vars/main.yml
  handlers/main.yml
  templates/sudoers.j2
  templates/motd.j2
  main_playbook.yaml
```

## Important Files

`tasks/main.yml`

Contains the main automation steps such as package installation, user creation, sudoers configuration, SSH hardening, and MOTD setup.

`vars/main.yml`

Stores variables such as package names, user details, and the login message.

`handlers/main.yml`

Contains the SSH restart handler. The handler runs only when notified by a changed SSH configuration task.

`templates/sudoers.j2`

Creates sudo access rules for the users defined in variables.

`templates/motd.j2`

Creates the server login message from the `motd_message` variable.

## Variables Used

Example package variable:

```yaml
baseline_packages:
  - git
  - curl
  - vim
  - htop
  - net-tools
```

Example user variable:

```yaml
baseline_users:
  - name: devops
    shell: /bin/bash
    groups: wheel
```

Example MOTD variable:

```yaml
motd_message: |
  This server is managed by Ansible.
  Unauthorized access is prohibited.
```

## Example Playbook

```yaml
- name: Apply server baseline
  hosts: man
  become: yes
  roles:
    - server_baseline
```

## How To Run

From the project directory, first run a syntax check:

```bash
ansible-playbook -i inventory.ini server_baseline/main_playbook.yaml --syntax-check
```

Then run the playbook:

```bash
ansible-playbook -i inventory.ini server_baseline/main_playbook.yaml
```

## Interview Points

This role is useful for interview practice because it covers:

- Roles
- Variables
- Loops
- Templates
- Handlers
- Idempotency
- Sudoers safety
- SSH hardening
