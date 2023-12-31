[![CI](https://github.com/de-it-krachten/ansible-role-kickstart/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-kickstart/actions?query=workflow%3ACI)


# ansible-role-kickstart

<basic role description>



## Dependencies

#### Roles
None

#### Collections
None

## Platforms

Supported platforms

- Red Hat Enterprise Linux 7<sup>1</sup>
- Red Hat Enterprise Linux 8<sup>1</sup>
- Red Hat Enterprise Linux 9<sup>1</sup>
- CentOS 7
- RockyLinux 8
- RockyLinux 9
- OracleLinux 8
- OracleLinux 9
- AlmaLinux 8
- AlmaLinux 9
- SUSE Linux Enterprise 15<sup>1</sup>
- openSUSE Leap 15
- Debian 11 (Bullseye)
- Debian 12 (Bookworm)
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS
- Fedora 37
- Fedora 38

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# Validate kickstart file using validator
kickstart_validator: true

# Format for kickstart file (iso, disk, file, floppy)
kickstart_mode: iso

# Packages for creating & validating kickstart files/images
kickstart_packages:
  - pykickstart
  - genisoimage
</pre></code>




## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'kickstart'
  hosts: all
  become: 'yes'
  tasks:
    - name: Include role 'kickstart'
      ansible.builtin.include_role:
        name: kickstart
</pre></code>
