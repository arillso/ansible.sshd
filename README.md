# Ansible Role: sshd

## Description

This role provides secure ssh-client and ssh-server configurations.  It is intended to be compliant with the [DevSec SSH  Baseline](https://github.com/dev-sec/ssh-baseline).

## Installation

```
$ ansible-galaxy install arillso.sshd
```

## Requirements

None

## Role Variables

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
|`network_ipv6_enable` | false |true if IPv6 is needed|
|`ssh_client_cbc_required` | false |true if CBC for ciphers is required. This is usually only necessary, if older M2M mechanism need to communicate with SSH, that don't have any of the configured secure ciphers enabled. CBC is a weak alternative. Anything weaker should be avoided and is thus not available.|
|`ssh_server_cbc_required` | false |true if CBC for ciphers is required. This is usually only necessary, if older M2M mechanism need to communicate with SSH, that don't have any of the configured secure ciphers enabled. CBC is a weak alternative. Anything weaker should be avoided and is thus not available.|
|`ssh_client_weak_hmac` | false |true if weaker HMAC mechanisms are required. This is usually only necessary, if older M2M mechanism need to communicate with SSH, that don't have any of the configured secure HMACs enabled.|
|`ssh_server_weak_hmac` | false |true if weaker HMAC mechanisms are required. This is usually only necessary, if older M2M mechanism need to communicate with SSH, that don't have any of the configured secure HMACs enabled.|
|`ssh_client_weak_kex` | false |true if weaker Key-Exchange (KEX) mechanisms are required. This is usually only necessary, if older M2M mechanism need to communicate with SSH, that don't have any of the configured secure KEXs enabled.|
|`ssh_server_weak_kex` | false |true if weaker Key-Exchange (KEX) mechanisms are required. This is usually only necessary, if older M2M mechanism need to communicate with SSH, that don't have any of the configured secure KEXs enabled.|
|`ssh_server_ports` | ['22'] |ports on which ssh-server should listen|
|`ssh_client_port` | '22' |port to which ssh-client should connect|
|`ssh_listen_to` | ['0.0.0.0'] |one or more ip addresses, to which ssh-server should listen to. Default is all adresseses, but should be configured to specific addresses for security reasons!|
|`ssh_host_key_files` | [] |Host keys for sshd. If empty ['/etc/ssh/ssh_host_rsa_key', '/etc/ssh/ssh_host_ecdsa_key', '/etc/ssh/ssh_host_ed25519_key'] will be used, as far as supported by the installed sshd version|
|`ssh_client_alive_interval` | 600 | specifies an interval for sending keepalive messages |
|`ssh_client_alive_count` | 3 | defines how often keep-alive messages are sent |
|`ssh_permit_tunnel` | false | true if SSH Port Tunneling is required |
|`ssh_remote_hosts` | [] | one or more hosts and their custom options for the ssh-client. Default is empty. See examples in `defaults/main.yml`.|
|`ssh_empty_passwords` | false | false when user not logged in wehn not password set. |
|`ssh_allow_root_with_key` | false | false to disable root login altogether. Set to true to allow root to login via key-based mechanism.|
|`ssh_allow_tcp_forwarding` | false | false to disable TCP Forwarding. Set to true to allow TCP Forwarding.|
|`ssh_allow_agent_forwarding` | false | false to disable Agent Forwarding. Set to true to allow Agent Forwarding.|
|`ssh_use_pam` | false | false to disable pam authentication.|
|`ssh_deny_users` | '' | if specified, login is disallowed for user names that match one of the patterns.|
|`ssh_allow_users` | '' | if specified, login is allowed only for user names that match one of the patterns.|
|`ssh_deny_groups` | '' | if specified, login is disallowed for users whose primary group or supplementary group list matches one of the patterns.|
|`ssh_allow_groups` | '' | if specified, login is allowed only for users whose primary group or supplementary group list matches one of the patterns.|
|`ssh_print_motd` | false | false to disable printing of the MOTD|
|`ssh_print_last_log` | false | false to disable display of last login information|
|`sftp_enabled` | true | false to disable sftp configuration|
|`sftp_chroot_dir` | /home/%u | change default sftp chroot location|
|`ssh_client_roaming` | false | enable experimental client roaming|
|`sshd_moduli_minimum` | 2048 | remove Diffie-Hellman parameters smaller than the defined size to mitigate logjam|
|`ssh_challengeresponseauthentication` | false | Specifies whether challenge-response authentication is allowed (e.g. via PAM) |
|`ssh_client_password_login` | false | `true` to allow password-based authentication with the ssh client |
|`ssh_server_password_login` | false | `true` to allow password-based authentication with the ssh server |
|`ssh_banner` | `false` | `true` to print a banner on login |
|`ssh_client_hardening` | `true` | `false` to stop harden the client |
|`ssh_client_port` | `'22'` | Specifies the port number to connect on the remote host. |
|`ssh_compression` | `false` | Specifies whether compression is enabled after the user has authenticated successfully. |
|`ssh_max_auth_retries` | `2` | Specifies the maximum number of authentication attempts permitted per connection. |
|`ssh_print_debian_banner` | `false` | `true` to print debian specific banner |
|`ssh_server_enabled` | `true` | `false` to disable the opensshd server |
|`ssh_server_hardening` | `true` | `false` to stop harden the server |
|`ssh_server_match_group` | '' | Introduces a conditional block.  If all of the criteria on the Match line are satisfied, the keywords on the following lines override those set in the global section of the config file, until either another Match line or the end of the file. |
|`ssh_server_match_user` | '' | Introduces a conditional block.  If all of the criteria on the Match line are satisfied, the keywords on the following lines override those set in the global section of the config file, until either another Match line or the end of the file. |
|`ssh_server_permit_environment_vars` | `false` | `true` to specify that ~/.ssh/environment and environment= options in ~/.ssh/authorized_keys are processed by sshd |
|`ssh_use_dns` | `false` | Specifies whether sshd should look up the remote host name, and to check that the resolved host name for the remote IP address maps back to the very same IP address. |
|`ssh_server_revoked_keys` | [] | a list of revoked public keys that the ssh server will always reject, useful to revoke known weak or compromised keys.|

## Dependencies

None

## Example Playbook

```yml
- hosts: all
  roles:
     - arillso.sshd
```

## Changelog

### 1.1

* add option when no password set by login
* add restart sshd by port change
* default value when ansible_ssh_port not set

### 1.0

* Initial release


## Author

* [Simon Bärlocher](https://sbaerlocher.ch)
 
## License

This project is under the MIT License. See the [LICENSE](https://sbaerlo.ch/licence) file for the full license text.

## Copyright

(c) 2017, Simon Bärlocher