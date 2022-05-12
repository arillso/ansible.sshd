# Ansible Role: sshd

[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-sshd-blue.svg?style=popout-square)](https://galaxy.ansible.com/skriptfabrik/sshd) [![Ansible Role](https://img.shields.io/ansible/role/d/59443.svg?style=popout-square)](https://galaxy.ansible.com/skriptfabrik/sshd)

## Description

This role provides secure ssh-client and ssh-server configurations. It is intended to be compliant with the [DevSec SSH Baseline](https://github.com/dev-sec/ssh-baseline).  
The role is a fork of [arillso.sshd](https://github.com/arillso/ansible.sshd).

## Installation

```bash
ansible-galaxy install skriptfabrik.sshd
```

## Requirements

None

## Role Variables

### ssh_ipv6_enable

true if IPv6 is needed

```yml
ssh_ipv6_enable: '{{ network_ipv6_enable | default(false) }}' # sshd + ssh
```

### ssh_server_enabled

true if sshd should be started and enabled

```yml
ssh_server_enabled: true # sshd
```

### ssh_use_dns

true if DNS resolutions are needed, look up the remote host name, defaults to false from 6.8, see: http://www.openssh.com/txt/release-6.8

```yml
ssh_use_dns: false # sshd
```

### ssh_compression

true or value if compression is needed

```yml
ssh_compression: false # sshd
```

### ssh_hardening

For which components (client and server) to generate the configuration for. Can be useful when running against a client without an SSH server.

```yml
ssh_client_hardening: true # ssh
ssh_server_hardening: true # sshd
```

### ssh_client_password_login

If true, password login is allowed

```yml
ssh_client_password_login: false # ssh
ssh_server_password_login: false # sshd
```

### ssh_server_ports

ports on which ssh-server should listen

```yml
ssh_server_ports: ['22'] # sshd
```

### ssh_client_port

port to which ssh-client should connect

```yml
ssh_client_port: '22' # ssh
```

### ssh_listen_to

one or more ip addresses, to which ssh-server should listen to. Default is empty, but should be configured for security reasons!

```yml
ssh_listen_to: ['0.0.0.0'] # sshd
```

### ssh_host_key_files

Host keys to look for when starting sshd.

```yml
ssh_host_key_files: [] # sshd
```

### ssh_max_auth_retries

Specifies the maximum number of authentication attempts permitted per connection. Once the number of failures reaches half this value, additional failures are logged.

```yml
ssh_max_auth_retries: 2
```

### ssh_client_alive_interval

```yml
ssh_client_alive_interval: 300 # sshd
```

### ssh_client_alive_count

```yml
ssh_client_alive_count: 3 # sshd
```

### ssh_permit_tunnel

Allow SSH Tunnels

```yml
ssh_permit_tunnel: false
```

### ssh_remote_hosts

Hosts with custom options. # ssh

```yml
ssh_remote_hosts: []
```

#### # Example

```yml
ssh_remote_hosts:
  - names: ['example.com', 'example2.com']
    options: ['Port 2222', 'ForwardAgent yes']
  - names: ['example3.com']
    options: ['StrictHostKeyChecking no']
```

### ssh_allow_root_with_key

Set this to "without-password" or "yes" to allow root to login

```yml
ssh_allow_root_with_key: 'no' # sshd
```

### ssh_allow_tcp_forwarding

false to disable TCP Forwarding. Set to true to allow TCP Forwarding.

```yml
ssh_allow_tcp_forwarding: false # sshd
```

### ssh_gateway_ports

false to disable binding forwarded ports to non-loopback addresses. Set to true to force binding on wildcard address.

Set to 'clientspecified' to allow the client to specify which address to bind to.

```yml
ssh_gateway_ports: false # sshd
```

### ssh_allow_agent_forwarding

false to disable Agent Forwarding. Set to true to allow Agent Forwarding.

```yml
ssh_allow_agent_forwarding: false # sshd
```

### ssh_pam_support

true if SSH has PAM support

```yml
ssh_pam_support: true
```

### ssh_use_pam

false to disable pam authentication.

```yml
ssh_use_pam: false # sshd
```

### ssh_google_auth

false to disable google 2fa authentication

```yml
ssh_google_auth: false # sshd
```

### ssh_pam_device

false to disable pam device 2FA input

```yml
ssh_pam_device: false # sshd
```

### ssh_gssapi_support

true if SSH support GSSAPI

```yml
ssh_gssapi_support: false
```

### ssh_kerberos_support

true if SSH support Kerberos

```yml
ssh_kerberos_support: true
```

### ssh_deny_users

if specified, login is disallowed for user names that match one of the patterns.

```yml
ssh_deny_users: '' # sshd
```

### ssh_allow_users

if specified, login is allowed only for user names that match one of the patterns.

```yml
ssh_allow_users: '' # sshd
```

### ssh_deny_groups

if specified, login is disallowed for users whose primary group or supplementary group list matches one of the patterns.

```yml
ssh_deny_groups: '' # sshd
```

### ssh_allow_groups

if specified, login is allowed only for users whose primary group or supplementary group list matches one of the patterns.

```yml
ssh_allow_groups: '' # sshd
```

### ssh_authorized_keys_file

change default file that contains the public keys that can be used for user authentication.

```yml
ssh_authorized_keys_file: '' # sshd
```

### ssh_trusted_user_ca_keys_file

specifies the file containing trusted certificate authorities public keys used to sign user certificates.

```yml
ssh_trusted_user_ca_keys_file: '' # sshd
```

### ssh_trusted_user_ca_keys

set the trusted certificate authorities public keys used to sign user certificates.

```yml
ssh_trusted_user_ca_keys: [] # sshd
```

#### Example

```yml
ssh_trusted_user_ca_keys:
  - 'ssh-rsa ... comment1'
  - 'ssh-rsa ... comment2'
```

### ssh_authorized_principals_file

specifies the file containing principals that are allowed. Only used if ssh_trusted_user_ca_keys_file is set.

```yml
ssh_authorized_principals_file: '' # sshd
```

#### Example

```yml
ssh_authorized_principals_file: '/etc/ssh/auth_principals/%u'
```

%h is replaced by the home directory of the user being authenticated, and %u is
replaced by the username of that user. After expansion, the path is taken to be
an absolute path or one relative to the user's home directory.

### ssh_authorized_principals

list of hashes containing file paths and authorized principals. Only used if ssh_authorized_principals_file is set.

```yml
ssh_authorized_principals: [] # sshd
```

#### Example

```yml
ssh_authorized_principals:
  - {
      path: '/etc/ssh/auth_principals/root',
      principals: ['root'],
      owner: '{{ ssh_owner }}',
      group: '{{ ssh_group }}',
      directoryowner: '{{ ssh_owner }}',
      directorygroup: '{{ ssh_group}}',
    }
  - {
      path: '/etc/ssh/auth_principals/myuser',
      principals: ['masteradmin', 'webserver'],
    }
```

### ssh_print_motd

false to disable printing of the MOTD

```yml
ssh_print_motd: false # sshd
```

### ssh_print_last_log

false to disable display of last login information

```yml
ssh_print_last_log: false # sshd
```

### ssh_banner

false to disable serving /etc/ssh/banner.txt before authentication is allowed

```yml
ssh_banner: false # sshd
```

### ssh_print_debian_banner

false to disable distribution version leakage during initial protocol handshake

```yml
ssh_print_debian_banner: false # sshd (Debian OS family only)
```

### ssh_sftp_enabled

true to enable sftp configuration

```yml
ssh_sftp_enabled: '{{ sftp_enabled | default(false) }}'
```

### ssh_sftp_chroot

false to disable sftp chroot

```yml
ssh_sftp_chroot: '{{ sftp_chroot | default(true) }}'
```

### ssh_sftp_chroot_dir

change default sftp chroot location

```yml
ssh_sftp_chroot_dir: "{{ sftp_chroot_dir | default('/home/%u') }}"
```

If true, password login for SFTP is allowed

```yml
ssh_server_sftp_password_login: false
ssh_server_sftp_password_login: true
```

### ssh_client_roaming

enable experimental client roaming

```yml
ssh_client_roaming: false
```

### ssh_server_match_user

list of hashes (containing user and rules) to generate Match User blocks for.

```yml
ssh_server_match_user: false # sshd
```

### ssh_server_match_group

list of hashes (containing group and rules) to generate Match Group blocks for.

```yml
ssh_server_match_group: false # sshd
```

### ssh_server_match_address

list of hashes (containing addresses/subnets and rules) to generate Match Address blocks for.

```yml
ssh_server_match_address: false # sshd
```

### ssh_server_permit_environment_vars

```yml
ssh_server_permit_environment_vars: false
```

### ssh_max_startups

maximum number of concurrent unauthenticated connections to the SSH daemon

```yml
ssh_max_startups: '10:30:100' # sshd
```

### ssh_ps53

```yml
ssh_ps53: 'yes'
```

### ssh_ps59

```yml
ssh_ps59: 'sandbox'
```

### ssh_macs

```yml
ssh_macs: []
```

### ssh_ciphers

```yml
ssh_ciphers: []
```

### ssh_kex

```yml
ssh_kex: []
```

### ssh_macs_53_default

```yml
ssh_macs_53_default:
  - hmac-ripemd160
  - hmac-sha1
```

### ssh_macs_59_default

```yml
ssh_macs_59_default:
  - hmac-sha2-512
  - hmac-sha2-256
  - hmac-ripemd160
```

### ssh_macs_66_default

```yml
ssh_macs_66_default:
  - hmac-sha2-512-etm@openssh.com
  - hmac-sha2-256-etm@openssh.com
  - umac-128-etm@openssh.com
  - hmac-sha2-512
  - hmac-sha2-256
```

### ssh_macs_76_default

```yml
ssh_macs_76_default:
  - hmac-sha2-512-etm@openssh.com
  - hmac-sha2-256-etm@openssh.com
  - umac-128-etm@openssh.com
  - hmac-sha2-512
  - hmac-sha2-256
```

### ssh_ciphers_53_default

```yml
ssh_ciphers_53_default:
  - aes256-ctr
  - aes192-ctr
  - aes128-ctr
```

### ssh_ciphers_66_default

```yml
ssh_ciphers_66_default:
  - chacha20-poly1305@openssh.com
  - aes256-gcm@openssh.com
  - aes128-gcm@openssh.com
  - aes256-ctr
  - aes192-ctr
  - aes128-ctr
```

### ssh_kex_59_default

```yml
ssh_kex_59_default:
  - diffie-hellman-group-exchange-sha256
```

### ssh_kex_66_default

```yml
ssh_kex_66_default:
  - curve25519-sha256@libssh.org
  - diffie-hellman-group-exchange-sha256
```

### ssh_custom_selinux_dir

directory where to store ssh_password policy

```yml
ssh_custom_selinux_dir: '/etc/selinux/local-policies'

sshd_moduli_file: '/etc/ssh/moduli'
sshd_moduli_minimum: 2048
```

### ssh_challengeresponseauthentication

disable ChallengeResponseAuthentication

```yml
ssh_challengeresponseauthentication: false
```

### ssh_server_revoked_keys

a list of public keys that are never accepted by the ssh server

```yml
ssh_server_revoked_keys: []
```

### ssh_hardening_enabled

Set to false to turn the role into a no-op. Useful when using
the Ansible role dependency mechanism.

```yml
ssh_hardening_enabled: true
```

### ssh_custom_options

Custom options for SSH client configuration file

```yml
ssh_custom_options: []
```

### sshd_custom_options

Custom options for SSH daemon configuration file

```yml
sshd_custom_options: []
```

## Dependencies

None

## Example Playbook

```yml
- hosts: all
  roles:
    - skriptfabrik.sshd
```

## Author

- [Simon Bärlocher](https://sbaerlocher.ch)
- [Frank Giesecke](https://github.com/FrankGiesecke)

## License

This project is under the MIT License.

## Copyright

(c) 2022, skriptfabrik GmbH
