<p><img src="https://static-www.elastic.co/v3/assets/bltefdd0b53724fa2ce/blt77c2da6e0198746e/620ac24e6662ca0a6f617114/icon-agent-32-color.svg" alt="elastic agent logo" title="agent" align="right" height="60" /></p>

# Ansible Role: Elastic Agent

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Ansible Role](https://img.shields.io/badge/ansible%20role-joedissmeyer.ansible_role_elastic_agent-blue.svg)](https://galaxy.ansible.com/joedissmeyer/ansible_role_elastic_agent)

## Description

Deploys and configures a Fleet managed [Elastic Agent](https://www.elastic.co/elastic-agent/) to target VM instances using Ansible. The purpose of this role is to quickly and efficiently deploy Elastic Agents in your infrastructure. Minimal variables are used to achieve this goal.

This role performs the following tasks:

1. Downloads the gzip binary of the Elastic Agent to the the remote target `/tmp` directory.
2. Extracts the gzip package.
3. Installs the Elastic Agent in Fleet managed mode.
4. Cleans up the downloaded and extracted files.

## Requirements

- Ansible >= 2.12 (It might work on previous versions.)
- Install the role, `ansible-galaxy install joedissmeyer.ansible_role_elastic_agent`.
- Only tested on RHEL/CentOS/Fedora Linux hosts. But this should work on Debian-based instances.
- An Elastic Agent policy and [Fleet enrollment token](https://www.elastic.co/guide/en/fleet/current/fleet-enrollment-tokens.html) must already be configured in your cluster. See official documentation for guidance.

## Role Variables

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file as well as in table below.

Several variables reference the official configuration items needed by Logstash to function. More details about every possible environment variable for Logstash can be found in the [official docs](https://www.elastic.co/guide/en/logstash/master/index.html).

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `elastic_agent_version` | '8.4.1' | The version of Logstash to download and install. |
| `agent_download_source` | https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-{{ elastic_agent_version }}-linux-x86_64.tar.gz | URL to download the Elastic Agent gzip package from. Relies on the `elastic_agent_version` variable to be set. |
| `fleet_server_insecure` | true | Define whether or not the Fleet server is listening on insecure HTTP/HTTPS (i.e. using self-signed certificates) or is secure. This setting is not yet fully implemented and is present for future enhancements of this role. |
| `fleet_server_host` | elk-fleet-server | The fleet server to register the agent to. This variable can be a hostname or IP address. |
| `fleet_server_port` | 8220 | The destination port for the Elasticsearch ingest endpoint. This is used in logstash pipelines. |

## Examples

Simply include the role in your playbook and modify variables as needed.

First, make sure the role is installed.
You can install via Ansible Galaxy or via requirements file.
To install using Galaxy,

```shell
$ ansible-galaxy install joedissmeyer.ansible_role_elastic_agent
```

Or using a requirements file:

```
roles:
- name: joedissmeyer.ansible_role_elastic_agent
  src: https://github.com/joedissmeyer/ansible-role-elastic-agent.git
```

You may include any variable overrides in `vars` as needed.
Note: It would be wise to place the Fleet enrollment token in an encrypted vars file using Ansible Vault.

```
- hosts: all
  name: Install Elastic Agent
  become: yes
  gather_facts: True
  tags:
    - agent
  roles:
  - role: joedissmeyer.ansible_role_elastic_agent
  vars:
    elastic_agent_version: "8.4.1"
    elastic_agent_enrollment_token: 'my_very_long_enrollment_token'
    fleet_server_host: 10.10.10.123
```

## License

This project is licensed under the Apache 2.0 license. See [LICENSE](/LICENSE) for more details.