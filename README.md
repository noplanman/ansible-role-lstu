# Ansible Role for Lstu (Let's Shorten That URL)

[![Build Status][travis-build-status]][travis-tests]

---

:rocket: Development has moved to **[git.feneas.org]**.

(The repository on GitHub is only a mirror, so fork on Feneas to contribute. No registration needed, just sign in with your GitHub account.)

---

This role installs and configures Lstu on Debian/Ubuntu servers.
Find out more about [Lstu], created by [Luc Didry].

This role will automatically install a service that will start when the server boots up.
It will figure out which service manager is being used automatically too.

## Requirements

Using this role doesn't install Nginx or Apache as a reverse proxy, you need to do that yourself!
Take a look at the [example configurations].

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

There are a few mandatory and many optional values. Check all possible variables in [`defaults/main.yml`][defaults].

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
    port: 5432
    user: "DBUSER"
    pwd: "DBPASSWORD"
    max_connections: 1
lstu_mysqldb:
    database: "lstu"
    host: "localhost"
    port: 3306
    user: "DBUSER"
    pwd: "DBPASSWORD"
    max_connections: 5
lstu_ban_min_strike: 3
lstu_ban_blacklist: []
lstu_ban_whitelist: []
lstu_piwik:
    url: "http://piwik.example.com"
    idsite: "1"
minion:
    enabled: no,
    db_path: "minion.db"
    pgdb:
        database: "lstu_minion"
        host: "localhost"
        port: 5432
        user: "DBUSER"
        pwd: "DBPASSWORD"
    mysqldb:
        database: "lstu_minion"
        host: "localhost"
        port: 3306
        user: "DBUSER"
        pwd: "DBPASSWORD"
lstu_ldap:
    uri: "ldaps://ldap.example.org"
    user_tree: "ou=users,dc=example,dc=org"
    bind_dn: "uid=ldap_user,ou=users,dc=example,dc=org"
    bind_pwd: "secr3t"
    user_attr: "uid"
    user_filter: "(!(uid=ldap_user))"
lstu_htpasswd: "lstu.passwd"
lstu_session_duration: 3600
lstu_max_redir: 2
lstu_spam_blacklist_regex: "foo|bar"
lstu_spam_path_blacklist_regex: "foo|bar"
lstu_spam_whitelist_regex: "foo|bar"
lstu_skip_spamhaus: no
lstu_safebrowsing_api_key: ""
lstu_memcached_servers: []
lstu_csp: "default-src 'none'; script-src 'self'; style-src 'self'; img-src 'self' data:; font-src 'self'; form-action 'self'; base-uri 'self'"
lstu_x_frame_options: "DENY"
lstu_x_content_type_options: "nosniff"
lstu_x_xss_protection: "1; mode=block"
lstu_log_creator_ip: no
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

[travis-build-status]: https://travis-ci.org/noplanman/ansible-role-lstu.svg?branch=master "Travis-CI Build Status"
[travis-tests]: https://travis-ci.org/noplanman/ansible-role-lstu "Travis-CI Tests"
[git.feneas.org]: https://git.feneas.org/noplanman/ansible-role-lstu "Ansible Role Lstu on Feneas"
[Lstu]: https://framagit.org/luc/lstu "Lstu on Framagit"
[Luc Didry]: https://framagit.org/u/luc "Luc on Framagit"
[example configurations]: https://framagit.org/luc/lstu/blob/master/utilities/ "Example configurations"
[defaults]: https://git.feneas.org/noplanman/ansible-role-lstu/blob/master/defaults/main.yml "Default variables"
