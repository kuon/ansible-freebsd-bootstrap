
# THIS REPOSITORY HAS MOVED

New URL: https://git.goyman.com/kuon/ansible-freebsd-bootstrap

Why I moved everything out of GitHub:

https://github.com/kuon/WhyILeftGithub/blob/main/README.md

----


# THIS REPOSITORY HAS MOVED

New URL: https://git.goyman.com/kuon/ansible-freebsd-bootstrap

Why I moved everything out of GitHub:

https://github.com/kuon/WhyILeftGithub/blob/main/README.md

----


# THIS REPOSITORY HAS MOVED

New URL: https://git.goyman.com/kuon/ansible-freebsd-bootstrap

Why I moved everything out of GitHub:

https://github.com/kuon/WhyILeftGithub/blob/main/README.md

----

FreeBSD-Bootstrap
=================

Bootstrap a FreeBSD host.

This role is split into two phase because the first phase is carried
as `root` and the second phase is a configuration phase (idempotent) which
is carried as the non root superuser.

### Phase 1

- connect as `root`
- install python
- install sudo
- allow ttyless sudo
- create a user named `boostrap_username` and give him passwordless sudo power
- copy the ssh key at path `bootstrap_sshkey` to the newly created superuser

### Phase 2

- connect as newly created superuser
- disable ssh root login
- remove root password
- enable ntp
- enable daily auto update
- setup the machine to use ssmtp (optional)
- install runit (without replacing init)
- install rsyslog



Role Variables
--------------

- `bootstrap_username`, the name of the superuser to create, default to `admin`
- `boostrap_sshkey`, the path to the ssh key to add to the superuser, default
  to `~/.ssh/id_rsa.pub`
- `bootstrap_phase`, the bootstrap phase to execute

You may enable ssmtp by setting the following variables:

- `bootstrap_ssmtp_server`, smtp server, can have a port number like
  smtp.gmail.com:587
- `bootstrap_ssmtp_root`, the email of the root user (will get all emails)
- `bootstrap_ssmtp_user`, smtp username
- `bootstrap_ssmtp_pass`, smtp password
- `bootstrap_ssmtp_domain`, the root domain to use as sender
- `bootstrap_ssmtp_tls`, set to true to use TLS


Role tags
---------

The following tags are supported to execute only one part of phase 2.

- `bootstrap_password`, disable root login
- `bootstrap_ntp`, install ntp
- `bootstrap_autoupdate`, setup autoupdate
- `bootstrap_ssmtp`, setup ssmtp (ssmtp configuration must also be set)
- `bootstrap_runit`, setup runit
- `bootstrap_fstab`, setup fstab (mount linux proc)
- `bootstrap_packages`, install base packages
- `bootstrap_locale`, set default locale to UTF-8
- `bootstrap_syslog`, setup rsyslog instead of syslog

Example Playbook
----------------

You need two playbooks to use this role, the first one, that should only be
executed once:


### Phase 1

    ---
    - hosts: servers
      remote_user: root
      gather_facts: false # Should be false as python is not installed yet

      roles:
        - {role: kuon.freebsd-bootstrap, bootstrap_phase: 1}

### Phase 2

    ---
    - hosts: servers
      sudo: true
      remote_user: admin # Should be the same as bootstrap_username

      roles:
        - {role: kuon.freebsd-bootstrap, bootstrap_phase: 2}


License
-------

MIT

Author Information
------------------

Nicolas Goy (@kuon)
