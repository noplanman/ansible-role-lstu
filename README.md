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

Set if Lstu should be kept up to date. (default: no)

```
lstu_keep_updated: no
```

If the admin password should be saved in a hashed format. *(`lstu_adminpwd` must always be the clear-text password!)*
 
```
lstu_hash_adminpwd: yes
```

There are a few mandatory and many optional values. Check all possible variables in `defaults/main.yml`.

```
# Required!
lstu_working_dir: "/var/www/example.com"
lstu_listen: "http://127.0.0.1:8080"    # Or an array, if multiple addresses.
lstu_contact: "admin@example.com"
lstu_secrets: ["array", "of", "random", "secrets"]

# Optional
lstu_proxy: no
lstu_adminpwd: "sup3r-secr3t"
lstu_theme: "default"
lstu_page_offset: 10
lstu_url_length: 8
lstu_provis_step: 5
lstu_provisioning: 100
lstu_prefix: "/"
lstu_allowed_domains: []
lstu_fixed_domain: "example.org"
lstu_db_type: "sqlite"
lstu_db_path: "lstu.db"
lstu_pgdb:
    database: "lstu"
    host: "localhost"
    user: "DBUSER"
    pwd: "DBPASSWORD"
lstu_mysqldb:
    database: "lstu"
    host: "localhost"
    user: "DBUSER"
    pwd: "DBPASSWORD"
lstu_ban_min_strike: 3
lstu_ban_whitelist: []
lstu_piwik:
    url: "http://piwik.example.com"
    idsite: "1"
minion:
    enabled: no,
    db_path: "minion.db"
lstu_ldap:
    uri: "ldaps://ldap.example.org"
    user_tree: "ou=users,dc=example,dc=org"
    bind_dn: ",ou=users,dc=example,dc=org"
    bind_user: "uid=ldap_user"
    bind_pwd: "secr3t"
    user_filter: "!(uid=ldap_user)"
lstu_htpasswd: "lstu.passwd"
lstu_session_duration: 3600
lstu_spam_blacklist_regex: "foo|bar"
lstu_spam_whitelist_regex: "foo|bar"
lstu_skip_spamhaus: no
```

## Role Tags

Each part of the setup has a tag.

```
lstu:install
lstu:site
lstu:service
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

## Tests

Docker is used to test the role with different operating systems.
Unfortunately this doesn't work with boot2docker for MacOS. My current workaround is to have a vagrant machine with docker installed, from which I call the tests. (Yes, a virtual container within a virtual machine...)

*(from `.travis.yml`)*
```bash
$ wget -O ${PWD}/tests/test.sh https://gist.githubusercontent.com/noplanman/40e96f31ee2301469769d4236aff40e2/raw/
$ chmod +x ${PWD}/tests/test.sh
$ distro=ubuntu1604 ${PWD}/tests/test.sh
$ distro=ubuntu1404 ${PWD}/tests/test.sh
$ distro=debian9 ${PWD}/tests/test.sh
$ distro=debian8 ${PWD}/tests/test.sh
```

## License

MIT
