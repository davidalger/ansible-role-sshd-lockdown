# Ansible Role: sshd-lockdown

[![Build Status](https://travis-ci.org/davidalger/ansible-role-sshd-lockdown.svg?branch=master)](https://travis-ci.org/davidalger/ansible-role-sshd-lockdown)

Replaces sshd config on CentOS / RHEL with a locked down sshd config including the following practices:

* `PermitRootLogin` is disabled.
* `PasswordAuthentication` is disabled.
* `sshusers` group is added and `sshd` configured such that only members of this group will be authorized.

## Requirements

None.

## Role Variables

    sshd_lockdown_config_template: sshd_config

Change this to specify an alternate template to use for the `sshd_config` file deployed to the server.

    sshd_additional_config_lines: []

Add lines of additional custom config to `sshd` service.

    sshd_sftp_subsystem: /usr/libexec/openssh/sftp-server

Variable for specifying alternate subsystem for use with sftp.

    sshd_access_users:
      - someotheruser
      - another_user
      - unprivileged_ssh_suer

List of users added to the `sshusers` group for access to the system.

## Dependencies

None.

## Example Playbook

    - hosts: web-servers
      roles:
        - { role: davidalger.sshd_lockdown }

## Example Playbook with Legacy Admin

For use on servers managed by Rackspace, the legacy `rack` user must be detected and added to the `sshusers` group and allowed an exception allowing it to use password authentication.

    - hosts: all
      vars:
        sshd_pass_auth_exception: true
        sshd_pass_auth_exception_user: rack

      roles:
        - { role: davidalger.sshd-lockdown }

## License

This work is licensed under the MIT license. See LICENSE file for details.

## Author Information

This role was created in 2016 by [David Alger](http://davidalger.com/) with contributions from [Matt Johnson](https://github.com/mttjohnson/).
