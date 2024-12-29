[![CI](https://github.com/de-it-krachten/ansible-role-kickstart/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-kickstart/actions?query=workflow%3ACI)


# ansible-role-kickstart

Create kickstart file for automatic installation of RedHat and derivatives
Kickstart file can be plain or within an image like iso or floppy



## Dependencies

#### Roles
None

#### Collections
None

## Platforms

Supported platforms

- Red Hat Enterprise Linux 8<sup>1</sup>
- Red Hat Enterprise Linux 9<sup>1</sup>
- RockyLinux 8
- RockyLinux 9
- OracleLinux 8
- OracleLinux 9
- AlmaLinux 8
- AlmaLinux 9
- SUSE Linux Enterprise 15<sup>1</sup>
- Debian 11 (Bullseye)
- Debian 12 (Bookworm)
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS
- Ubuntu 24.04 LTS
- Fedora 40
- Fedora 41

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# Prepare host for kickstart
kickstart_prepare: true

# Validate kickstart file using validator
kickstart_validator: true

# Format for kickstart file (iso, file, fdd/floppy)
kickstart_mode: iso

kickstart_bios_mode: uefi

# Define where to write kickstart image to
kickstart_path: /tmp

# List of OS packages for creating & validating kickstart files/images
kickstart_os_packages:
  RedHat:
    - genisoimage
    - dosfstools
  Debian:
    - genisoimage
    - dosfstools

# List of pip/pypi packages for creating & validating kickstart files/images
kickstart_pip_packages:
  - pykickstart
  - passlib

# Name of the VM
kickstart_hostname: localhost

# Kickstart template to use
kickstart_template: ks-rhel{{ kickstart_version }}.cfg.j2
kickstart_template_version: RHEL{{ kickstart_version }}

# Lookup dict for sources
kickstart_sources:
  'centos8':
    baseos: https://vault.centos.org/centos/8/BaseOS/x86_64/os
    appstream: https://vault.centos.org/centos/8/AppStream/x86_64/os
  'rocky8':
    baseos: https://download.rockylinux.org/pub/rocky/8/BaseOS/x86_64/os
    appstream: https://download.rockylinux.org/pub/rocky/8/AppStream/x86_64/os
  'rocky9':
    baseos: https://download.rockylinux.org/pub/rocky/9/BaseOS/x86_64/os
    appstream: https://download.rockylinux.org/pub/rocky/8/AppStream/x86_64/os
  'oraclelinux8':
    baseos: https://yum.oracle.com/repo/OracleLinux/OL8/baseos/latest/x86_64
    appstream: https://yum.oracle.com/repo/OracleLinux/OL8/appstream/x86_64
  'oraclelinux9':
    baseos: https://yum.oracle.com/repo/OracleLinux/OL9/baseos/latest/x86_64
    appstream: https://yum.oracle.com/repo/OracleLinux/OL9/appstream/x86_64
  'almalinux8':
    baseos: https://repo.almalinux.org/almalinux/8/BaseOS/x86_64/os
    appstream: https://repo.almalinux.org/almalinux/8/AppStream/x86_64/os
  'almalinux9':
    baseos: https://repo.almalinux.org/almalinux/9/BaseOS/x86_64/os
    appstream: https://repo.almalinux.org/almalinux/9/AppStream/x86_64/os

# Environment
kickstart_lang: "en_US.UTF-8"
kickstart_timezone: "Europe/Amsterdam"

# Users
kickstart_users:
  root_password: root
  ansible_user: ansible
  ansible_password: ansible

# Network settings
kickstart_network:
  hostname: "{{ kickstart_hostname }}"
  bootproto: dhcp
  device: enp0s3
  # bootproto: static
  # ip: 192.168.1.100
  # gateway: 192.168.1.1
  # dns1: 192.168.1.1
  # netmask: 255.255.255.0

# LVM settings
kickstart_lvm:
  vg: rootvg
  pe_size: 32768
  lv:
    - { name: lv_root, size: 8, mp: /, fstype: xfs }
    - { name: lv_home, size: 1, mp: /home, fstype: xfs }
    - { name: lv_tmp, size: 2, mp: /tmp, fstype: xfs }
    - { name: lv_usrlocal, size: 1, mp: /usr/local, fstype: xfs }
    - { name: lv_var, size: 2, mp: /var, fstype: xfs }
    - { name: lv_varlog, size: 1, mp: /var/log, fstype: xfs }
    - { name: lv_swap, size: 2, mp: swap, fstype: swap }

# Final action after installation
# kickstart_final_action: "reboot --eject"
kickstart_final_action: "poweroff"
</pre></code>




## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'kickstart'
  hosts: all
  become: 'yes'
  vars:
    kickstart_distros:
      - name: Rocky
        version: 8
        mode: iso
      - name: Rocky
        version: 9
        mode: iso
      - name: OracleLinux
        version: 8
        mode: iso
      - name: OracleLinux
        version: 9
        mode: iso
      - name: Rocky
        version: 8
        mode: fdd
      - name: Rocky
        version: 9
        mode: fdd
      - name: OracleLinux
        version: 8
        mode: fdd
      - name: OracleLinux
        version: 9
        mode: fdd
  tasks:
    - name: Include role 'kickstart'
      ansible.builtin.include_role:
        name: kickstart
      vars:
        kickstart_distro: '{{ item.name }}'
        kickstart_version: '{{ item.version }}'
        kickstart_mode: '{{ item.mode }}'
      loop: '{{ kickstart_distros }}'
</pre></code>
