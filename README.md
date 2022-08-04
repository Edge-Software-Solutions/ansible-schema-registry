# Confluent Schema Registry

Ansible role to install and configure [Confluent Schema Registry - Community]

[Confluent Schema Registry - Community] provides a serving layer for your metadata. It provides 
a RESTful interface for storing and retrieving your [avro], [json], and
[protobuf] schemas. It stores a versioned history of all schemas based on a specified subject 
name strategy, provides multiple compatibility settings and allows evolution of 
schemas according to the configured compatibility settings and expanded support 
for these schema types. It provides serializers that plug into [apache kafka] clients 
that handle schema storage and retrieval for [apache kafka] messages that are sent in 
any of the supported formats.

## Supported Platforms

- Ubuntu 18.04.x
- Ubuntu 20.04.x

## Requirements

- [apache kafka]
- Java 11 / 17

The below Apache Kafka role from Ansible Galaxy can be used if one is
needed.

```sh
ansible-galaxy install sleighzy.kafka
```

Ansible 2.9.16 or 2.10.4 are the minimum required versions to work around an
issue with certain kernels that have broken the `systemd` status check. The
error message "`Service is in unknown state`" will be output when attempting to
start the service via the Ansible role and the task will fail. The service will
start as expected if the `systemctl start` command is run on the physical host.
See <https://github.com/ansible/ansible/issues/71528> for more information.

## Role Variables

| Variable                                       | Default                                           | Comments                                                                      |
|------------------------------------------------|---------------------------------------------------|-------------------------------------------------------------------------------|
| schema_registry_community_version              | 2.13                                              |                                                                               |
| schema_registry_apt_repo_key_url               | https://packages.confluent.io/deb/7.2/archive.key |                                                                               |
| schema_registry_listeners_string               | http://0.0.0.0:8081                               |                                                                               |
| schema_registry_kafka_bootstrap_servers_string | SSL://server1:9093,SSL://server2:9093             |                                                                               |
| schema_registry_kafka_security_protocol        | SSL                                               |                                                                               |
| schema_registry_kafka_schema_topic             | _schemas                                          |                                                                               |
| schema_registry_config_params                  |                                                   | A key-value dictionary that will be templated into schema-registry.properties |

## Starting and Stopping Confluent Schema Registry services using systemd

- The Confluent Schema Registry service can be started via: `systemctl start confluent-schema-registry`
- The Confluent Schema Registry service can be stopped via: `systemctl stop confluent-schema-registry`
 
### Ports

| Port | Description                   |
|------|-------------------------------|
| 8081 | Schema Registry listener port |

### Directories and Files

| Directory / File        |                                                         |
|-------------------------|---------------------------------------------------------|
| Schema Registry service | `/lib/systemd/system/confluent-schema-registry.service` |

## Example Playbook

Add the below to a playbook to run those role against hosts belonging to the
`schema-registry-nodes` group.

```yaml
- hosts: schema-registry-nodes
  roles:
    - edgesoftwaresolutions.schema-registry
```

## License

[![MIT license]](https://lbesson.mit-license.org/)

[Confluent Schema Registry - Community]: https://docs.confluent.io/platform/current/schema-registry/index.html
[apache kafka]: https://kafka.apache.org/
[avro]: https://avro.apache.org/
[json]: https://json-schema.org/
[protobuf]: https://developers.google.com/protocol-buffers/
[mit license]: https://img.shields.io/badge/License-MIT-blue.svg
