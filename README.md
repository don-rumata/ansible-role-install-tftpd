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
- Add rule 4 firewalld (https://gist.github.com/shamil/d2fb2bfa92b769c17df84706904d9ee7).

## Known issue

- Error:

```none
"module_stdout": "\r\nTraceback (most recent call last):\r\n  File \"/home/ansible-test-user/.ansible/tmp/ansible-tmp-1626521566.63-15344-118601205128264/AnsiballZ_ini_file.py\", line 102, in <module>\r\n    _ansiballz_main()\r\n  File \"/home/ansible-test-user/.ansible/tmp/ansible-tmp-1626521566.63-15344-118601205128264/AnsiballZ_ini_file.py\", line 94, in _ansiballz_main\r\n    invoke_module(zipped_mod, temp_path, ANSIBALLZ_PARAMS)\r\n  File \"/home/ansible-test-user/.ansible/tmp/ansible-tmp-1626521566.63-15344-118601205128264/AnsiballZ_ini_file.py\", line 40, in invoke_module\r\n    runpy.run_module(mod_name='ansible.modules.files.ini_file', init_globals=None, run_name='__main__', alter_sys=True)\r\n  File \"/usr/lib/python3.8/runpy.py\", line 207, in run_module\r\n    return _run_module_code(code, init_globals, run_name, mod_spec)\r\n  File \"/usr/lib/python3.8/runpy.py\", line 97, in _run_module_code\r\n    _run_code(code, mod_globals, init_globals,\r\n  File \"/usr/lib/python3.8/runpy.py\", line 87, in _run_code\r\n    exec(code, run_globals)\r\n  File \"/tmp/ansible_ini_file_payload_c_pldhft/ansible_ini_file_payload.zip/ansible/modules/files/ini_file.py\", line 342, in <module>\r\n  File \"/tmp/ansible_ini_file_payload_c_pldhft/ansible_ini_file_payload.zip/ansible/modules/files/ini_file.py\", line 322, in main\r\n  File \"/tmp/ansible_ini_file_payload_c_pldhft/ansible_ini_file_payload.zip/ansible/modules/files/ini_file.py\", line 156, in do_ini\r\n  File \"/usr/lib/python3.8/encodings/ascii.py\", line 26, in decode\r\n    return codecs.ascii_decode(input, self.errors)[0]\r\nUnicodeDecodeError: 'ascii' codec can't decode byte 0xc2 in position 1032: ordinal not in range(128)\r\n"
```

For fix it, use: `LC_CTYPE: en_US.UTF-8` or `LC_CTYPE: C` in `environment`. Like:

```yaml
  - name: Install tftpd
    hosts: all
    strategy: free
    serial:
      - "100%"
    roles:
      - role: don_rumata.ansible_role_install_tftpd
        environment:
          LC_CTYPE: en_US.UTF-8
```

[license-image]: https://img.shields.io/github/license/don-rumata/ansible-role-install-tftpd.svg
[license-url]: https://opensource.org/licenses/Apache-2.0

[ansible-galaxy-image]: https://img.shields.io/badge/ansible_galaxy-don__rumata.ansible__role__install__tftpd-blue.svg
[ansible-galaxy-url]: https://galaxy.ansible.com/don_rumata/ansible_role_install_tftpd

[ansible-galaxy-quality-image]: https://img.shields.io/ansible/quality/55766
