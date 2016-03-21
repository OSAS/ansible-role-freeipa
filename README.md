Ansible module used by OSAS to manage freeipa.

[![Build Status](https://travis-ci.org/OSAS/ansible-role-freeipa.svg?branch=master)](https://travis-ci.org/OSAS/ansible-role-httpd)


# Variables

## Passwords

In order to setup and integrate freeipa,  2 passwords need to be set.

The first one is `kerberos_admin_password`, to be used for admin of the kerberos server.
It will be used for the admin@EXAMPLE.ORG principal.

The second one is the `directory_admin_password` for the LDAP directory.

Both need to be protected

## Topology

Since the role is supposed to deploy and integrate freeipa on all servers,
a few settings can be used to 

By default, the role will detect the domain name of the current master 
and capitalize it to create the realm. For example, ipa.example.org will 
become EXAMPLE.ORG. The variable `freeipa_domain` can be used to override that
if the detection is not correct. This cannot be changed after setup.

The topology requires to have the name of the master, with the variable
`freeipa_master`. One or more replicas be setup with `freeipa_replicas`. The list
of replicas can be changed later to add and remove replica if the need arise.

# Integration in a playbook

The role is written in such a way that it will deploy a freeipa master, one or more replicas and
enroll the others systems in the realm.

One example playbook to use this would be this:

```
- hosts: all
  roles:
  - role: freeipa
    freeipa_master: freeipa.example.org
    freeipa_replicas:
    - freeipa-slave01.example.org
```

# Distribution support

For now, this is mostly tested on RH-like distribution. While Debian-like are supported in theory, this 
is not tested. And non Linux (or less mainstream distributions) are currently not supported upstream.
