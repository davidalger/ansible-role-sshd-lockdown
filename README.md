# Ansible Role: sshd-lockdown

[![Build Status](https://travis-ci.org/davidalger/ansible-role-sshd-lockdown.svg?branch=master)](https://travis-ci.org/davidalger/ansible-role-sshd-lockdown)

Replaces sshd config on CentOS / RHEL. This includes the following (review included configuration template for complete details)

This role replaces the sshd config using a template file and ansible variables. The defaults provide a locked down sshd config where password authentication is disabled for SSH users, and users must be a part of the `sshusers` group in order to be granted the ability to connect and authenticate over SSH. 

* `PermitRootLogin` is disabled.
* `PasswordAuthentication` is disabled.
* An `sshusers` group is added and `sshd` configured such that only members of this group will be authorized.
* For use on servers managed by Rackspace, the `rack` user is detected and added to the `sshusers` group if present.

## TODO

Research https://www.ssh.com/ssh/sshd_config

## Requirements

None.

## Role Variables

    sshd_lockdown_config_template: sshd_config

Change this to specify an alternate template to use for the `sshd_config` file deployed to the server.

    sshd_additional_config_lines: []

Add lines of additional custom config to `sshd` service.

    sshd_sftp_subsystem: /usr/libexec/openssh/sftp-server

Variable for specifying alternate subsystem for use with sftp.

## Dependencies

None.

## Example Playbook

    - hosts: web-servers
      roles:
        - { role: davidalger.sshd_lockdown }

    - hosts: all
      vars:
        sshd_pass_auth_exception: true
        sshd_pass_auth_exception_user: rack

        sshd_access_users:
          - someotheruser
          - another_user
          - unprivileged_ssh_suer
      roles:
        - { role: davidalger.sshd-lockdown }

## License

This work is licensed under the MIT license. See LICENSE file for details.

## Author Information

This role was created in 2016 by [David Alger](http://davidalger.com/) with contributions from [Matt Johnson](https://github.com/mttjohnson/).
