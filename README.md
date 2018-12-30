[![Build Status](https://travis-ci.com/calvinbui/ansible-pip.svg?branch=master)](https://travis-ci.com/calvinbui/ansible-pip)
[![](https://img.shields.io/ansible/role/d/35859.svg)](https://galaxy.ansible.com/calvinbui/ansible_pip)

# Ansible pip

Installs the Python package manager `pip` based on the version provided or the version of Python that Ansible grabs as a fallback.

Also installs pip packages with any of the parameters provided by the pip module.

## Requirements

N/A

## Role Variables

`pip_version`: The version of Python to install (2 or 3). If left blank, it will default to what Ansible is using on the remote host. This means `3` if the `ansible_python.version.major` fact is `3`; and `2` if `2`

`pip_install_packages`: A list of packages to install with the pip module.  Set it to `[]` if no packages are required.

[All available options used in the pip module can be used](https://docs.ansible.com/ansible/2.7/modules/pip_module.html). Set it exactly the same as the pip module, e.g.

```
pip_install_packages:
  - name: virtualenv
  - name: pyyaml
    state: present
  - ...
```

Notes:
- `executable` by default uses the pip executable version being installed (a.k.a. `pip_version`). This can be overridden by providing the pip `executable`.
- `executable` will always attempt to use the setuptools for the version of Ansible running in the remote machine ([see this issue](https://github.com/ansible/ansible/issues/47361#issuecomment-431705748)). This role will ensure this is covered by following the table below:

| Local Python | Remote Python | Executable | Requirements                     |
|--------------|---------------|------------|----------------------------------|
| 2            | 2             | 2          | None                             |
| 2            | 2             | 3          | Installs setuptools for Python 2 |
| 2            | 3             | 3          | None                             |
| 2            | 3             | 2          | Installs setuptools for Python 3 |
| 3            | 2             | 2          | None                             |
| 3            | 2             | 3          | Installs setuptools for Python 2 |
| 3            | 3             | 3          | None                             |
| 3            | 3             | 2          | Installs setuptools for Python 3 |

## Dependencies

N/A

## Example Playbook


```yaml
- hosts: all
  become: true
  pre_tasks:
    - name: Update apt cache.
      apt:
        update_cache: true
        cache_valid_time: 600
      changed_when: false
  roles:
    - role: ansible-pip
```

## License

GPLv3

## Author Information

https://calvin.me
