# Ansible ansible-ufw role
[![GitHub License](https://img.shields.io/badge/license-MIT-lightgrey.svg)](https://github.com/trustedshops-public/ansible-ufw/blob/main/LICENSE)

> `ansible-ufw` is an [Ansible](http://www.ansible.com) role which:
>
> * installs ufw
> * configures ufw
> * configures ufw rules
> * configures service

This is a fork from [weareinteractive/ansible-ufw](https://github.com/weareinteractive/ansible-ufw)

## Installation

Using `requirements.yml`:

```yaml
- src: git+ssh://git@github.com:trustedshops/ansible-ufw.git
```

Using `git`:

```shell
$ git clone git@github.com:trustedshops/ansible-ufw.git
```

## Dependencies

* Ansible >= 2.10

## Variables

Here is a list of all the default variables for this role, which are also available in `defaults/main.yml`.

```yaml
---
# Start the service and enable it on system boot
ufw_enabled: true

# Reset all of the ufw rules
ufw_reset: false

# List of packages to install
ufw_packages: ["ufw"]

# The service name
ufw_service: ufw

# List of rules to be applied
# see https://docs.ansible.com/ansible/latest/collections/community/general/ufw_module.html for documentation
ufw_rules:
  - rule: allow
    to_port: 22

# Manage the configuration file
ufw_manage_config: false

# Configuration object passed to the configuration file
ufw_config:
  IPV6: "yes"
  DEFAULT_INPUT_POLICY: DROP
  DEFAULT_OUTPUT_POLICY: ACCEPT
  DEFAULT_FORWARD_POLICY: DROP
  DEFAULT_APPLICATION_POLICY: SKIP
  MANAGE_BUILTINS: "no"
  IPT_SYSCTL: /etc/ufw/sysctl.conf
  IPT_MODULES: ""

# Path to the configuration file
ufw_config_file: /etc/default/ufw

```

## Handlers

These are the handlers that are defined in `handlers/main.yml`.

```yaml
---

- name: reset ufw
  community.general.ufw:
    state: reset

- name: reload ufw
  community.general.ufw:
    state: reloaded
  when: ufw_enabled | bool

```


## Usage

This is an example playbook:

```yaml
# @see https://docs.ansible.com/ansible/latest/collections/community/general/ufw_module.html#examples
---

- hosts: all
  become: true
  roles:
    - weareinteractive.ufw
  vars:
    ufw_rules:
      # Set loggin
      - logging: "full"
      # Allow OpenSSH
      - rule: allow
        name: OpenSSH
      # Delete OpenSSH rule
      - rule: allow
        name: OpenSSH
        delete: true
      # Allow all access to tcp port 80
      - rule: allow
        to_port: '80'
        proto: tcp
    # Manage the configuration file
    ufw_manage_config: true
    # Configuration object passed to the configuration file
    ufw_config:
      IPV6: "yes"
      DEFAULT_INPUT_POLICY: DROP
      DEFAULT_OUTPUT_POLICY: ACCEPT
      DEFAULT_FORWARD_POLICY: DROP
      DEFAULT_APPLICATION_POLICY: SKIP
      MANAGE_BUILTINS: "no"
      IPT_SYSCTL: /etc/ufw/sysctl.conf
      IPT_MODULES: ""

```


## Testing

```shell
$ git clone https://github.com/weareinteractive/ansible-ufw.git
$ cd ansible-ufw
$ make test
```

## Contributing
In lieu of a formal style guide, take care to maintain the existing coding style. Add unit tests and examples for any new or changed functionality.

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

*Note: To update the `README.md` file please install and run `ansible-readme`:*

```shell
$ gem install ansible-readme
$ ansible-readme
```

## License
Copyright (c) We Are Interactive, Trusted Shops AG under the MIT license.
