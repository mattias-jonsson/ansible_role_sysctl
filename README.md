Ansible Role: ansible_role_sysctl
=========

An Ansible role that configures sysctl entries. The /etc/sysctl.conf is replaced with a template, removing any modifcations made. Files are created under /etc/sysctl.d with entries specified in role variables. A backup is created for any modified file in the format of \<filename\>.{{timestamp}}.bkp.
This role supports the following Linux distributions:

<ul>
<li>Fedora 34
<li>CentOS 7/8
<li>Red Hat Enterprise Linux 7/8
<li>Debian 10/11
<li>Ubuntu 18/20/22
</ul>

Requirements
------------

This role has no dependencies besides what is included in Ansible Core.

Role Variables
--------------

Available variables are listed below, along with default values where applicable (see defaults/main.yml):

| Variable | Required | Default | Comments |
| -------- | -------- | ------- | -------- |
| `filename` | No | [] | List of filenames. Name of the file to be created under /etc/sysctl.d, the order number will be prepended to this name. Each file can have their own setting, see example playbook below. |
| `order` | No | | Integer to be prepended to the filename, sysctl will read the files in order and the last value will win if specified more than once. |
| `settings` | No | | Settings to be added to the file, see example playbook below. |

Dependencies
------------

This role has no external dependencies.

Example Playbook
----------------

This example created two files, 97_customentries.conf and 98_swappiness.conf under /etc/sysctl.d with the content specified under settings.

    - hosts: servers

      vars:
        ansible_role_sysctl_d:
        - filename: customentries
            order: 97
            settings:
              kernel.core_pattern: /var/log/core.%e.%p
              net.ipv6.conf.all.disable_ipv6: 1
              net.ipv6.conf.default.disable_ipv6: 1
              vm.min_free_kbytes: 2097152
              kernel.numa_balancing: 0
              fs.nr_open: 1048576
              vm.max_map_count: 262140
              vm.overcommit_memory: 2
              vm.overcommit_ratio: 100
              vm.nr_hugepages: 0
              vm.zone_reclaim_mode: 0

        - filename: swappiness
            order: 98
            settings:
              vm.wappiness: 10

      roles:
         - role: ansible_role_sysctl

License
-------

MIT

Author Information
------------------

Mattias Jonsson