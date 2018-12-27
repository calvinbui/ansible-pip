# Ansible pip


Installs `pip` based on the version of Python that Ansible grabs.

Also installs pip packages with any of the parameters provided by the pip module.

## Requirements

N/A

## Role Variables

`pip_version`: The version of Python to install (2 or 3). If left blank, it will default to what Ansible is using ont he remote host. This means `pip3` if the `ansible_python.version.major` is `3` and `pip` if it is anything else (most likely `2`)

`pip_install_packages`: A list of packages to install with the pip module.  Set it to `[]` if no packages are required.

[All available options used in the pip module can be used here as well](https://docs.ansible.com/ansible/2.7/modules/pip_module.html). Available options:

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

Notes:
- `executable` by default uses the Python that Ansible uses on the remote host. This means `pip3` if the `ansible_python.version.major` is `3` and `pip` if it is anything else (most likely `2`). This can be overridden by providing the full path to the `pip` executable. **It does not match `pip_version`**.
- `name` is omitted if `requirements` is provided. They are mutually exclusive
- `executable` is omitted if `virtualenv` is defined.
- `executable`

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
