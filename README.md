# [Ansible role openvpn](#ansible-role-openvpn)

Install and configure openvpn server or client on your system.

|GitHub|GitLab|Downloads|Version|
|------|------|---------|-------|
|[![github](https://github.com/buluma/ansible-role-openvpn/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-openvpn/actions)|[![gitlab](https://gitlab.com/shadowwalker/ansible-role-openvpn/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-openvpn)|[![downloads](https://img.shields.io/ansible/role/d/buluma/openvpn)](https://galaxy.ansible.com/buluma/openvpn)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-openvpn.svg)](https://github.com/buluma/ansible-role-openvpn/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-openvpn/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true

  tasks:
    - name: Create openvpn server
      ansible.builtin.include_role:
        name: ansible-role-openvpn
      vars:
        openvpn_role: "server"

    - name: Copy certificates and keys from the server to the client
      ansible.builtin.copy:
        src: /etc/openvpn/easy-rsa/pki/{{ item }}
        dest: /etc/openvpn/client/{{ item | basename }}
        mode: "0640"
        remote_src: true
      loop:
        - ca.crt
        - issued/client.crt
        - private/client.key
        - ta.key

    - name: Create openvpn client
      ansible.builtin.include_role:
        name: ansible-role-openvpn
      vars:
        openvpn_role: "client"
        openvpn_client_server: "127.0.0.1"
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-openvpn/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: false

  roles:
    - role: buluma.bootstrap
    - role: buluma.epel
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-openvpn/blob/master/defaults/main.yml):

```yaml
---
# defaults file for openvpn

# You can setup both a client and a server using this role.
# Use `server` or `client` for `openvpn_role`.

openvpn_role: server

# If you are configuring a client, setup these variables:
# openvpn_role: client
# openvpn_client_server: vpn.example.com
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-openvpn/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Build Status GitHub](https://github.com/buluma/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.epel](https://galaxy.ansible.com/buluma/epel)|[![Build Status GitHub](https://github.com/buluma/ansible-role-epel/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-epel/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-epel/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-epel)|

## [Context](#context)

This role is part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-openvpn/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[Debian](https://hub.docker.com/r/buluma/debian)|all|
|[Ubuntu](https://hub.docker.com/r/buluma/ubuntu)|all|

The minimum version of Ansible required is 2.12, tests have been done on:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them on [GitHub](https://github.com/buluma/ansible-role-openvpn/issues).

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-openvpn/blob/master/LICENSE).

## [Author Information](#author-information)

[buluma](https://buluma.github.io/)

