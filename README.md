Ansible Role Apt
=========

[![Build Status](https://travis-ci.org/Turgon37/ansible-apt.svg?branch=master)](https://travis-ci.org/Turgon37/ansible-apt)
[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)
[![Ansible Role](https://img.shields.io/badge/ansible%20role-Turgon37.apt-blue.svg)](https://galaxy.ansible.com/Turgon37/apt/)

## Description

:grey_exclamation: Before using this role, please know that all my Ansible roles are fully written and accustomed to my IT infrastructure. So, even if they are as generic as possible they will not necessarily fill your needs, I advice you to carrefully analyse what they do and evaluate their capability to be installed securely on your servers.

This roles configure the APT common settings, APT repositories and GPG keys.

## Requirements

Require Ansible >= 2.4

### Dependencies

If you use the zabbix monitoring profile you will need the role [ansible-zabbix-agent](https://github.com/Turgon37/ansible-zabbix-agent)

## OS Family

This role is available for Debian

## Features

At this day the role can be used to :

  * configure apt settings (in conf.d)
  * configure apt repositories
  * install apt gpg keys
  * create apt pinning
  * enable unattended upgrades
  * configure listchanges
  * monitoring items for
    * Zabbix
  * [local facts](#facts)

## Role Variables

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file as well as in table below. To see default values please refer to this file.

| Name                                                      | Types/Values          | Description                                                                                           |
| ----------------------------------------------------------| ----------------------|------------------------------------------------------------------------------------------------------ |
| `apt__facts`                                              | Boolean               | Install the local fact script                                                                         |
| `apt__use_cached_updates_list`                            | Boolean               | If true, apt will produce files with packages that require update after each apt-update operation     |
| `apt__monitoring`                                         | String                | The name of the monitoring "profile" to use. Available 'zabbix')                                      |
| `apt__repositories`                                       | List of Dict          | see official apt_repository module                                                                    |
| `apt__repositories_purge`                                 | Boolean               | If true, all non ansible managed repositories will be removed                                         |
| `apt__keys`                                               | List of Dict          | see official apt_key module                                                                           |
| `apt__proxy`                                              | Dict                  | Configure the http proxy for APT, format of dict  => {host: 'host', port: 8080, https: True}          |
| `apt__periodic_enabled`                                   | Boolean               | Enable periodic cron task for APT                                                                     |
| `apt__periodic_update_package_lists`                      | Integer               | If > 0, automatic apt-get update will be performed by periodic task every n-days                      |
| `apt__periodic_unattended_upgrade`                        | Integer               | If > 0, automatic apt-get upgrade for security updates will be performed by periodic task every n-days|
| `apt__periodic_download_upgradeable_packages`             | Integer               | If > 0, automatic apt-get download will be performed by periodic task every n-days                    |
| `apt__periodic_*`                                         |                       | See others periodic settings in the [defaults/main.yml](defaults/main.yml)                            |
| `apt__unattended_upgrades_mail`                           | String                | Specify the mail address of the recipient for report messages                                         |
| `apt__unattended_upgrades_automatic_reboot`               | Boolean               | If true, automatic reboot will be performed when needed (kernel upgrade..)                            |
| `apt__unattended_upgrades_automatic_reboot_time`          | String                | A specific time that you allow the servers to be reboot                                               |
| `apt__unattended_upgrades_package_blacklist`              | List of String        | List of package name (regexp allowed) to blacklist from auto upgrade                                  |
| `apt__unattended_upgrades_*`                              |                       | See others unattended_upgrades settings in the [defaults/main.yml](defaults/main.yml)                 |
| `apt__listchanges_enabled`                                | Boolean               | Enable listchanges feature                                                                            |
| `apt__listchanges_configurations_(global/group/host)`     |                       | Change listchanges configurations (see below for example)                                             |
| `apt__configurations_purge`                               | Boolean               | If true, all non ansible managed configuration will be removed                                        |
| `apt__configurations_purge_whitelist_(global/group/host)` | List of file names    | List of configuration filenames to exclude from purge                                                 |
| `apt__configurations_(global/group/host)                  | Dict of configurations| Deploy apt configurations in apt.conf.d                                                               |
| `apt__repositories_(global/group/host)`                   | List of repositories  | List of repositories to configure                                                                     |
| `apt__pins_(global/group/host)`                           | Dict of pins          | Dict of pinning configurations                                                                        |

### APT configuration

Each APT configuration consist in a name, a priority and a content. The priority must be included in the name and the content will be put as it is in the final file.

```
apt__configurations_group:
  00cdrom:
    content: |-
      Acquire::cdrom {
        mount "/media/cdrom";
      };
      Dir::Media::MountPath "/media/cdrom";
      APT::Authentication::TrustCDROM "true";
```

Will create a file named 00cdrom with the content above.


### APT pinning

Each pinning configuration must follow the behavour explained in apt_preferences(5)

The key of the dict represent the "name" of the pinning, it's an abstract name, it will simply be used to create the configuration file in apt preferences directory. In addition, this name let you override any pinning configuration from a level of inventory in a lower level.

Each pinning must contains at least the following attributes

```
[filename]:
  explanation: a simple string that describe the pinning usage
  package: string/list of packages name to which the pinning will apply, default to *
  priority: the priority integer
```

Then, in a pin configuration you can one of the three types of packages filtering, in *release, version, origin*

* to pin using version

```
perl:
  explanation: pinning on version
  package: perl
  priority: 100
  version: 5.8*
```

* to pin using origin

```
all-debian:
  explanation: pinning on origin
  package: *
  priority: 999
  origin: Debian
```

* to pin using release

The following keys are available, *archive (a), codename (n), release_version(v), component(c), origin(o), label (l)*

```
bc:
  explanation: pinning on release
  package: *
  priority: 50
  archive: unstable
```

By default, the name of the pinning setting is used as archive release parameter, so the two following bloc are equivalents

```
stable:
  explanation: pinning on stable release
  package: *
  priority: 50
  archive: stable
```

```
stable:
  explanation: pinning on stable release
  package: *
  priority: 50
```

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

## Example

### Playbook

Use it in a playbook as follows:

```yaml
- hosts: all
  roles:
    - turgon37.apt
```

### Inventory

To use this role create or update your playbook according the following example :

```
apt__repositories:
  - name: raspbian
    repo: deb http://mirrordirector.raspbian.org/raspbian/ stretch main contrib non-free rpi
    codename: stretch
apt__unattended_upgrades_mail: 'admin@example.com'
apt__periodic_update_package_lists: True
apt__periodic_verbose: 1
```

* Specify custom configuration

```
apt__configurations_global:
  70recommends:
    content: |-
      APT::Install-Recommends "1";
      APT::Install-Suggests "0";
  80no-languages:
    content: |-
      Acquire::Languages "none";
```

* Configure listchanges settings

```
apt__listchanges_enabled: true
apt__listchanges_configurations_global:
  apt.confirm: 'true'
  apt.email_address: '{{ email_admin }}'
  apt.which: changelogs
  cmdline.frontend: pager
```

* Add stable repository on an oldstable host and pin it to prevent dist-upgrade behaviour

```
apt__repositories:
  - name: raspbian
    repo: deb http://mirrordirector.raspbian.org/raspbian/ stretch main contrib non-free rpi
    codename: stretch
apt__pins_host:
  stretch:
    explanation: Pin stretch to prevent dist-upgrade behaviour
    codename: stretch
    priority: 400
```
