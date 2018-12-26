# Ansible pip


Installs `pip` based on the version of Python that Ansible grabs.

Also installs pip packages with any of the parameters provided by the pip module.

## Requirements

N/A

## Role Variables

`pip_python_version`: The version of Python to install (2 or 3).

`pip_install_packages`: A list of packages to install with the pip module. [All available options used in the pip module can be used here as well](https://docs.ansible.com/ansible/2.7/modules/pip_module.html).

Available options:

```
name
version
requirements
virtualenv
virtualenv_command
virtualenv_python
virtualenv_site_packages
extra_args
umask
state
executable
```

By default it will use the Python executable that Ansible uses. This can be overridden if required by specifying `pip` or `pip3`. **It does not match `pip_python_version`**.

Examples:

```yaml
pip_install_packages:
  # by itself installs latest
  - virtualenv
  - docker-py

  # with name and state
  - name: pyyaml
    state: present
  - name: awscli
    state: absent

  # with virtualenv
  - name: requests
    virtualenv: /tmp/venv

  # with version
  - name: numpy
    version: 1.15.4

  # with requirements file
  - requirements: /tmp/requirements.txt
```

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
