# Ansible Role: NSO Local Installation

Ansible role for Cisco [Network Services Orchestrator](https://developer.cisco.com/docs/nso/#!nso-fundamentals) (NSO) [Local](https://developer.cisco.com/docs/nso/#!getting-and-installing-nso/local-vs-system-installation) installation

## Overview

* Execute NSO Local installation
  *  **(Optional)** Include external YANG files
* Create NSO runtime environment
  * **(Optional)** Customize the runtime configuration
  * **(Optional)** Automatically build and load NED's
  * **(Optional)** Create, customize, and load NSO NETSIM's
  * **(Optional)** Configure NSO CDB

### Role Restrictions

* The use of this role assumes the user can obtain the appropriate binaries as noted in [Prerequisites](#prerequisites)
  * Evaluation copies for NSO & various NSO NED's can be found on [DevNet](https://developer.cisco.com/docs/nso/#!getting-and-installing-nso/download-your-nso-free-trial-installer-and-cisco-neds)

### Prerequisites

* Ansible
  * Ansible >= 2.9.1 (might work with earlier versions, but not tested)
* NSO
  * Python 3
  * Operating System requirements as described on [DevNet](https://developer.cisco.com/docs/nso/#!getting-and-installing-nso/requirements) (Java + Ant)
    * **NOTE:** If ```Java``` or ```Ant``` binaries are installed in a location other than the default, you can change the respective ```java_binary``` or ```ant_binary``` variables in [vars](vars)
    * **NOTE:** Assumes the target host has ```build-essentials``` (gcc, make, etc.) and ```xsltproc``` installed
  * NSO Signed Binary (*.signed.bin)
    * The NSO signed binary **must** be placed within the (```files```) folder
  * NED Signed Binaries (*.signed.bin)
      * The NED signed binaries **must** be placed within the (```files```) folder

### Validated NSO Versions

* 5.5

## Role Variables

All variables which can be overridden are stored in various files within [defaults/main](defaults/main)

### Setup ([setup.yml](defaults/main/setup.yml))

These variables are directly related to running the _NSO Local installation_ and _NSO Runtime_ environment

| Name | Default | Description |
| ---- | ------- | ----------- |
| `nso_root_dir` | "~/nso" | Root path to directory where the NSO installation, runtime, external YANG files, and NETSIM will reside.<br /><br /> In other words, ```nso_install_dir```, ```nso_runtime_dir```, ```nso_yang_dir```, and ```nso_netsim_dir``` are all derived from this variable within [vars/main.yml](vars/main.yml). |
| `nso_yang_files` | [] | External YANG files (i.e.; those not included as part of NSO installation) to be installed. Examples provided in the file. |

### NETSIM ([netsim-vars.yml](defaults/main/netsim-vars.yml))

These variables are directly related to the creation of NETSIM devices

| Name | Default | Description |
| ---- | ------- | ----------- |
| `nso_netsim` | [] | List of NETSIM devices to be created. Allows specifying the number of NETSIM devices per type, number of GigabitEthernet, and number of TenGigabitEthernet interfaces. |

### Runtime Configuration ([nso-runtime-config.yml](defaults/main/nso-runtime-config.yml))

These variables are directly related to the parameters used to construct the NSO runtime configuration, `ncs.conf`, abstracted from the various 'Configuration Parameters' found in the NSO Manual Pages.

| Name | Default | Description |
| ---- | ------- | ----------- |
| `nso_config_hide_group` | [] | Corresponds to the `/ncs-config/hide-group` section. |
| `nso_config_rollback` | {} | Corresponds to the `/ncs-config/rollback` section. |
| `nso_config_cli` | {} | Corresponds to the `/ncs-config/cli` section. |
| `nso_config_webui_tcp` | {} | Corresponds to the `/ncs-config/webui/transport/tcp` section. |
| `nso_config_webui_ssl` | {} | Corresponds to the `/ncs-config/webui/transport/ssl` section. |
| `nso_config_restconf` | {} | Corresponds to the `/ncs-config/restconf` section. |

### CDB Configuration ([nso-config.yml](defaults/main/nso-config.yml))

These variables are directly related to configuration than can be applied to the NSO CDB

| Name | Default | Description |
| ---- | ------- | ----------- |
| `nso_customers` | [] | Corresponds to the `/ncs:customers/ncs:customer` configuration. |

## Sample Playbook

```yaml
---
- name: NSO Local Installation
  hosts: all
  gather_facts: yes
  vars:
    nso_root_dir: "~/nso"
    nso_yang_files:
      - name: "ietf-routing-types.yang"
        uri: "https://raw.githubusercontent.com/YangModels/yang/master/standard/ietf/RFC/ietf-routing-types%402017-12-04.yang"
    nso_netsim:
      - type: iosxr
        type_count: 3
        type_count_gige: 2
        type_count_tengige: 2
    nso_config_hide_group:
      - name: debug
    nso_config_rollback:
      enabled: true
      directory: ./logs
      history_size: 500
      rollback_numbering: rolling
    nso_config_cli:
      rollback_numbering: rolling
    nso_config_webui_ssl:
      enabled: true
      ip: 0.0.0.0
      port: 8888
      key_file: ${NCS_DIR}/var/ncs/webui/cert/host.key
      cert_file: ${NCS_DIR}/var/ncs/webui/cert/host.cert
    nso_customers:
      - id: Disneyland
      - id: Universal
  tasks:
    - name: Setup NSO
      include_role:
        name: dbono711.ansible_nso_local_install
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
