# Ansible Role: Install Tftpd

[![License][license-image]][license-url] [![Ansible Galaxy][ansible-galaxy-image]][ansible-galaxy-url] [![Ansible Galaxy Quality][ansible-galaxy-quality-image]][ansible-galaxy-url]

Install [tftpd](https://mirrors.edge.kernel.org/pub/software/network/tftp) for Linux.

## Work on

### Ansible Galaxy style

```yaml
  platforms:
    - name: Fedora
      versions:
        - 33
        - 34
    - name: Ubuntu
      versions:
        - bionic
        - focal
    - name: Debian
      version:
        - stretch
        - buster
        - oldstable
        - stable
        - testing
    - name: EL (CentOS)
      versions:
        - 8
    - name: opensuse
      vesrion:
        - 15.3
        - tumbleweed
```

## Requirements

None.

## Role Variables

```yaml
---
tftpd_directory: /var/lib/tftpboot
tftpd_user: tftp
tftpd_group: tftp
tftpd_chmod: u+rw,g+rw,o+rx

tftpd_address: 0.0.0.0
tftpd_port: 69
tftpd_options: --port-range 30000:32000 --verbose --verbose --verbose --secure
```

## HowTo

### How to install role

Over `ansible-galaxy`:

```bash
ansible-galaxy install don_rumata.ansible_role_install_tftpd
```

Over `bash+git`:

```bash
mkdir -p "$HOME/.ansible/roles"
cd "$HOME/.ansible/roles"
git clone https://github.com/don-rumata/ansible-role-install-tftpd don_rumata.ansible_role_install_tftpd
```

## Example Playbooks

### I

Install latest stable `tftpd` on Linux:

`install-tftpd.yml`:

```yaml
- name: Install tftpd
  hosts: all
  strategy: free
  serial:
    - "100%"
  roles:
    - don_rumata.ansible_role_install_tftpd
  tasks:
```

`tftpd-inventory.ini`:

```ini
[tftpd]
172.16.10.10
```

```bash
ansible-playbook -i ./tftpd-inventory.ini ./install-tftpd.yml
```

### II

Install latest stable `tftpd` on Linux with custom root directory:

`install-tftpd.yml`:

```yaml
- name: Install tftpd
  hosts: all
  strategy: free
  serial:
    - "100%"
  roles:
    - role: don_rumata.ansible_role_install_tftpd
      tftpd_directory: /data/tftpboot
  tasks:
```

## License

Apache License, Version 2.0

## Author Information

[don Rumata](https://github.com/don-rumata)

## TODO

- Add tests.

[license-image]: https://img.shields.io/github/license/don-rumata/ansible-role-install-tftpd.svg
[license-url]: https://opensource.org/licenses/Apache-2.0

[ansible-galaxy-image]: https://img.shields.io/badge/ansible_galaxy-don__rumata.ansible__role__install__tftpd-blue.svg
[ansible-galaxy-url]: https://galaxy.ansible.com/don_rumata/ansible_role_install_tftpd

[ansible-galaxy-quality-image]: https://img.shields.io/ansible/quality/55672
