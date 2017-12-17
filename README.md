Ansible Role Apt
=========

:warning: This role is under development, some important (and possibly breaking) changes may happend. Don't use it in production level environments but you can eventually base your own role on this one :hammer:

:grey_exclamation: Before using this role, please know that all my Ansible role are fully written and accustomed to my IT infrastructure. So, even if they are as generic as possible they will not necessarily fill your needs, I advice you to carrefully analyse what they do and evaluate their capability to be installed securely on your servers.

**This roles configure the APT common settings, APT repositories and GPG keys.**

## Features

Currently this role provide the following features :

  * apt repository setting
  * apt gpg keys setting
  * unattended upgrade configuration
  * monitoring items for
    * Zabbix
  * [local facts](#facts)

## Requirements

### OS Family

This role is available for Debian only

### Dependencies

If you use the zabbix monitoring profile you will need the role [ansible-zabbix-common](https://github.com/Turgon37/ansible-zabbix-common)


## Role Variables

The variables that can be passed to this role and a brief description about them are as follows:

| Name                                          | Types/Values   | Description                                                                                           |
| ----------------------------------------------| ---------------|------------------------------------------------------------------------------------------------------ |
| apt__facts                                    | Boolean        | Install the local fact script                                                                         |
| apt__monitoring                               | String         | The name of the monitoring "profile" to use. Available 'zabbix')                                      |
| apt__repositories                             | List of Dict   | see official apt_repository module                                                                    |
| apt__keys                                     | List of Dict   | see official apt_key module                                                                           |
| apt__proxy                                    | Dict           | Configure the http proxy for APT, format of dict  => {host: 'host', port: 8080, https: True}          |
| apt__periodic_enabled                         | Boolean        | Enable periodic cron task for APT                                                                     |
| apt__periodic_update_package_lists            | Integer        | If > 0, automatic apt-get update will be performed by periodic task every n-days                      |
| apt__periodic_unattended_upgrade              | Integer        | If > 0, automatic apt-get upgrade for security updates will be performed by periodic task every n-days|
| apt__periodic_download_upgradeable_packages   | Integer        | If > 0, automatic apt-get download will be performed by periodic task every n-days                    |
| apt__unattended_upgrades_mail                 | String         | Specify the mail address of the recipient for report messages                                         |
| apt__unattended_upgrades_automatic_reboot     | Boolean        | If true, automatic reboot will be performed when needed (kernel upgrade..)                            |
| apt__unattended_upgrades_automatic_reboot_time| String         | A specific time that you allow the servers to be reboot                                               |
| apt__unattended_upgrades_package_blacklist    | List of String | List of package name (regexp allowed) to blacklist from auto upgrade                                  |

## Facts

By default the local fact are installed and expose the following variables :


```
ansible_local.apt:
    "packages_updates": [],
    "packages_dist_updates": [],
    "packages_security_updates": [],
    "packages_security_dist_updates": [],
    "updates": 0,
    "dist_updates": 0,
    "security_updates": 0,
    "security_dist_updates": 0,
    "has_updates": true,
    "has_dist_updates": true,
    "reboot_required": false,
    "update_last_success": 1513468567.5470324
}
```


## Example Playbook

To use this role create or update your playbook according the following example :


```
    - hosts: servers
      roles:
         - apt
      vars:
        apt__repositories:
          - name:     raspbian
            repo:     deb http://mirrordirector.raspbian.org/raspbian/ stretch main contrib non-free rpi
            codename: stretch
        apt__unattended_upgrades_mail: 'admin@example.com'
        apt__periodic_update_package_lists: True
        apt__periodic_verbose: 1
```


## License

MIT