# Ansible Role for Lstu (Let's Shorten That URL)

[![Build Status](https://travis-ci.org/noplanman/ansible-lstu.svg?branch=master)](https://travis-ci.org/noplanman/ansible-lstu)

Installs and configures Lstu on Debian/Ubuntu servers.
Find out more about Lstu [here](https://framagit.org/luc/lstu), created by [Luc Didry](https://framagit.org/u/luc).

This role will automatically install a service that will start when the server boots up.
It will figure out which service manager is being used automatically too.

## Requirements

Using this role doesn't install Nginx or Apache as a reverse proxy, you need to do that yourself!
An example configuration for Nginx can be found [here](https://framagit.org/luc/lstu/blob/master/utilities/lstu.nginx).

## Role Variables

Set the user/group that will be used to run Lstu. It makes sense to use the webserver user/group.

```
lstu_user: www-data
lstu_group: www-data
```

There are a few mandatory and many optional values. Check all possible variables in `defaults/main.yml`.

```
# Required!
lstu_working_dir: "/var/www/example.com"
lstu_listen: "http://127.0.0.1:8080"
lstu_contact: "admin@example.com"
lstu_secrets: ["array", "of", "random", "secrets"]
# Optional
lstu_theme: "default"
lstu_adminpwd: "sup3r-secr3t"
lstu_allowed_domains: []
```

## Role Tags

Each part of the setup has a tag.

```
lstu.install
lstu.site
lstu.service
```

## Dependencies

None.

## Example Playbook

```
# playbook.yml
---
- hosts: servers
  become: yes
  vars_files:
    - vars/main.yml
  roles:
    - { role: noplanman.lstu }
```
```
# vars/main.yml
---
lstu_working_dir: "/var/www/lstu.example.com"
lstu_listen: "http://127.0.0.1:8080"
lstu_contact: "admin@lstu.example.com"
lstu_secrets: ["Weiboe3a","Ahsahm0O","biPhah2u","Ki2teeph","Choihah4","ka7Oifia","aeT7eeb9","oYah4it7"]
lstu_adminpwd: "kohdah6DJieSh1CaTe1uWohKax9ahxeG"
```

## License

MIT
