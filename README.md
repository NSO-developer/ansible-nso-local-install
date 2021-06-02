# Ansible Role: NSO Local Installation

Ansible role for [Cisco Network Services Orchestrator](https://developer.cisco.com/docs/nso/#!nso-fundamentals) (NSO) [Local](https://developer.cisco.com/docs/nso/#!getting-and-installing-nso/local-vs-system-installation) installation

## Overview

Automates the installation of Cisco NSO using the "Local" installation method, including:

* Creation of NSO runtime environment
  * Ability to customize the runtime configuration
* Ability to include external YANG files
* Ability to automatically build and load NED's
* Ability to create, customize, and load NSO NETSIM's
* Ability to configure NSO

**[NOTE]** *The version of NSO that will be installed is based on the NSO binary noted in ```Prerequisites```*

**[NOTE]** *The version(s) of NED(s) that will be installed is based on the NED binaries noted in ```Prerequisites```*

### Role Restrictions

* The use of this role is supported on the following target hosts:
  * macOS (Darwin)
* The use of this role assumes the ability to obtain the appropriate NSO and NED binaries
  * Evaluation copies for NSO & various NSO NED's can be found on [DevNet](https://developer.cisco.com/docs/nso/#!getting-and-installing-nso/download-your-nso-free-trial-installer-and-cisco-neds)

### Validated NSO Versions

* 5.5

### Prerequisites

* Running this role
  * Ansible >= 2.9
* NSO
  * Python 3
  * Operating System requirements as described on [DevNet](https://developer.cisco.com/docs/nso/#!getting-and-installing-nso/requirements) (Java + Ant)
  * NSO Signed Binary
    * The NSO signed binary (*.signed.bin) must be placed within the (```files/nso```) folder
  * NSO NED Signed Binaries
      * The NED signed binaries (*.signed.bin) must be placed within the (```files/neds```) folder

## Role Variables

* All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml), highlighted in the table below


| Name           | Default | Description                        |
| -------------- | ------------- | -----------------------------------|
| `nso_root_dir` | "~/nso" | (String) Root path to directory where the NSO installation, runtime, External YANG files (Optional), and NETSIM (Optional) will reside. ```nso_install_dir```, ```nso_runtime_dir```, ```nso_yang_dir```, and ```nso_netsim_dir``` are all derived from this variable within [vars/main.yml](vars/main.yml). |
| `nso_yang_files` | [] | (List) External YANG files (i.e.; those not included as part of NSO installation) to be installed |
| `nso_netsim` | [] | (List) NETSIM environment and simulated devices to be installed. **NOTE: Each run of the playbook will wipe existing NETSIM environment** |

### Sample Playbook

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

### Local Testing

COMING SOON!

## Additional Resources

[Learning NSO](https://developer.cisco.com/docs/nso/#!learning-nso)

[Documentation Guides](https://developer.cisco.com/docs/nso/guides/)

## Authors

* Darren Bono - [darren.bono@att.net](mailto://darren.bono@att.net)

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE.md) for details
