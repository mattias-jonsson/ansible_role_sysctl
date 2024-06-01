Ansible Role: sysctl
=========

This Ansible role configures sysctl settings by overwriting the `/etc/sysctl.conf` file and creating additional configuration files under the `/etc/sysctl.d` directory as specified by the role variables.

This role supports the following Linux distributions:

- Enterprise Linux (RedHat, CentOS, Alma Linux OS, Rocky Linux etc.)
- Debian-based (Debian, Ubuntu etc.)

Role Variables
--------------

Available variables are listed below, along with default values where applicable (see defaults/main.yml):

| Variable                | Required | Default | Description                                                                                                                 |
|-------------------------|----------|---------|-----------------------------------------------------------------------------------------------------------------------------|
| `sysctl_config_backup`  | No       | true    | Indicates whether a backup of any modified configuration files should be created.                                           |
| `sysctl_d`              | Yes      | []      | A list of dictionaries, each representing a sysctl configuration. Each dictionary contains `filename`, `order`, and `settings`. See details below.  |

Each dictionary in the `sysctl_d` list includes the following keys:

| Key        | Required | Description                                                                                                         |
|------------|----------|---------------------------------------------------------------------------------------------------------------------|
| `filename` | Yes      | The name of the file to be created under `/etc/sysctl.d/`, prefixed with the `order` number.                        |
| `order`    | Yes      | An integer used as the prefix for `filename`. The sysctl command processes files in numerical order, with later values taking precedence. |
| `settings` | Yes      | A dictionary of sysctl parameters and their respective values.                                                      |
                                         |

Example Playbook
----------------

This example created two files, 97_customentries.conf and 98_swappiness.conf under /etc/sysctl.d with the content specified under settings.

    - hosts: servers

      vars:
        sysctl_config_backup: true
        sysctl_d:
        - filename: customentries
            order: 97
            settings:
              kernel.core_pattern: /var/log/core.%e.%p
              net.ipv6.conf.all.disable_ipv6: 1
              net.ipv6.conf.default.disable_ipv6: 1

        - filename: swappiness
            order: 98
            settings:
              vm.swappiness: 10

      roles:
         - role: sysctl

License
-------

MIT

Author Information
------------------

Mattias Jonsson
