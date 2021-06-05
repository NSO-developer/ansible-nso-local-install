# Ansible Role: NSO Local Installation

Ansible role for [Cisco Network Services Orchestrator](https://developer.cisco.com/docs/nso/#!nso-fundamentals) (NSO) [Local](https://developer.cisco.com/docs/nso/#!getting-and-installing-nso/local-vs-system-installation) installation

## Overview

Automates the installation of Cisco NSO using the "Local" installation method, including:

* Creation of NSO runtime environment
  * **(Optional)** Ability to customize the runtime configuration
* **(Optional)** Ability to include external YANG files
* **(Optional)** Ability to automatically build and load NED's
* **(Optional)** Ability to create, customize, and load NSO NETSIM's
* **(Optional)** Ability to configure NSO

**[NOTE]** *The version of NSO that will be installed is based on the NSO binary noted in [Prerequisites](#prerequisites)*

**[NOTE]** *The version(s) of NED(s) that will be installed is based on the NED binaries noted in [Prerequisites](#prerequisites)*

### Role Restrictions

* The use of this role is supported on the following target hosts:
  * macOS (Darwin)
* The use of this role assumes the ability to obtain the appropriate NSO and NED binaries
  * Evaluation copies for NSO & various NSO NED's can be found on [DevNet](https://developer.cisco.com/docs/nso/#!getting-and-installing-nso/download-your-nso-free-trial-installer-and-cisco-neds)

### Validated NSO Versions

* 5.5

### Prerequisites

* Ansible
  * Ansible >= 2.9.1 (might work with earlier versions, but not tested)
* NSO
  * Python 3
  * Operating System requirements as described on [DevNet](https://developer.cisco.com/docs/nso/#!getting-and-installing-nso/requirements) (Java + Ant)
  * NSO Signed Binary
    * The NSO signed binary (*.signed.bin) must be placed within the (```files/nso```) folder
  * NSO NED Signed Binaries
      * The NED signed binaries (*.signed.bin) must be placed within the (```files/neds```) folder

## Role Variables

All variables which can be configured or overridden are stored in various files within [defaults/main](defaults/main)

### Setup ([setup.yml](defaults/main/setup.yml))

These variables are directly related to the setup of the NSO Local installation and NSO Runtime environment

| Name | Default | Description |
| ---- | ------- | ----------- |
| `nso_root_dir` | "~/nso" | Root path to directory where the NSO installation, runtime, external YANG files, and NETSIM will reside.<br /><br /> In other words, ```nso_install_dir```, ```nso_runtime_dir```, ```nso_yang_dir```, and ```nso_netsim_dir``` are all derived from this variable within [vars/main.yml](vars/main.yml). |
| `nso_yang_files` | [] | External YANG files (i.e.; those not included as part of NSO installation) to be installed. Examples provided in the file. |

### NETSIM ([netsim-vars.yml](defaults/main/netsim-vars.yml))

These variables are directly related to the creation of various NETSIM devices

| Name | Default | Description |
| ---- | ------- | ----------- |
| `nso_netsim` | [] | List of NETSIM devices to be created. Allows specifying the number of NETSIM devices per type, number of GigabitEthernet, and number of TenGigabitEthernet interfaces. |

### Runtime Configuration ([nso-runtime-config.yml](defaults/main/nso-runtime-config.yml))

These variables are directly related to the parameters used to construct the NSO runtime configuration, `ncs.conf`, abstracted from the various 'Configuration Parameters' found in the NSO Manual Pages.

**Not all parameters may be present, but aim to be added over time.**

**Individual examples and/or defaults are provided in the file**

| Name | Default | Description |
| ---- | ------- | ----------- |
| `nso_config_hide_group` | [] | Corresponds to the `/ncs-config/hide-group` section. |
| `nso_config_rollback` | {} | Corresponds to the `/ncs-config/rollback` section. |
| `nso_config_cli` | {} | Corresponds to the `/ncs-config/cli` section. |
| `nso_config_webui_tcp` | {} | Corresponds to the `/ncs-config/webui/transport/tcp` section. |
| `nso_config_webui_ssl` | {} | Corresponds to the `/ncs-config/webui/transport/ssl` section. |

### CDB Configuration ([nso-config.yml](defaults/main/nso-config.yml))

These variables are directly related to various configuration to be applied to the NSO CDB using the Ansible `nso_config` module

**Not all parameters may be present, but aim to be added over time.**

**Individual examples and/or defaults are provided in the file**

| Name | Default | Description |
| ---- | ------- | ----------- |
| `nso_customers` | [] | Corresponds to the `/ncs:customers/ncs:customer` configuration. |

## Sample Playbook

Execute this role on 'localhost'

```yaml
---
- name: NSO Local Installation
  hosts: all
  gather_facts: yes
  tasks:
    - name: Setup NSO
      include_role:
        name: ansible-nso-local-install
      tags:
        - "always"
```

## Local Testing

COMING SOON!

## Additional Resources

[Learning NSO](https://developer.cisco.com/docs/nso/#!learning-nso)

[Documentation Guides](https://developer.cisco.com/docs/nso/guides/)

## Authors

* Darren Bono - [darren.bono@att.net](mailto://darren.bono@att.net)

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE.md) for details
