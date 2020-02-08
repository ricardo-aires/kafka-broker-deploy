# kafka Broker

Role that setup [Kafka Broker](https://kafka.apache.org/).

> The solutions provided was designed for Proof of Concepts. Hence, are not to be treated as production ready, especially because of the lack of Security settings.

## Requirements

This role was created using [Ansible 2.9](https://docs.ansible.com/ansible/2.9/) for macOS and tested using the [centos/7](https://app.vagrantup.com/centos/boxes/7) boxes for [Vagrant v.2.2.6](https://www.vagrantup.com/docs/index.html) with [VirtualBox](https://www.virtualbox.org/) as a Provider.

The [Ansible modules](https://docs.ansible.com/ansible/2.9/modules/modules_by_category.html) used in the role are:

- [package](https://docs.ansible.com/ansible/latest/modules/package_module.html#package-module)
- [group](https://docs.ansible.com/ansible/2.9/modules/group_module.html#group-module)
- [user](https://docs.ansible.com/ansible/2.9/modules/user_module.html#user-module)
- [file](https://docs.ansible.com/ansible/2.9/modules/file_module.html#file-module)
- [get_url](https://docs.ansible.com/ansible/2.9/modules/get_url_module.html#get_url-module)
- [unarchive](https://docs.ansible.com/ansible/2.9/modules/unarchive_module.html#unarchive-module)
- [template](https://docs.ansible.com/ansible/2.9/modules/template_module.html#template-module)
- [service](https://docs.ansible.com/ansible/2.9/modules/service_module.html#service-module)

> We are using `systemd` to control the service.

A [ZooKeeper Server](http://zookeeper.apache.org) is required. You can check our [zookeeper-deploy](http://github.com/ricardo-aires/zookeeper-deploy) or [use the one that comes with kafka](https://kafka.apache.org/quickstart) to get one up and running, or even check the [ZooKeeper Docker Official Image](https://hub.docker.com/_/zookeeper).

## Role Variables

The next variables are set in [defaults](./defaults/main.yml) in order to be [easily overwrite](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable) and fetch a different version:

- `kafka_user`: to setup details for the `kafka` user.
- `kafka_version`: version of the Kafka to be installed.
- `kafka_tarball_checksum`: if a different version is needed.

Some of the [Kafka configuration](https://kafka.apache.org/documentation/#configuration) can also be tweaked using variables which also are in [defaults](./defaults/main.yml) and optimize for a 3 node cluster.

You should, however, change the one with the list of the ZooKeeper Ensemble and the Heap Size of the Kafka Server:

```bash
kafka_zookeeper_connect: 192.168.33.11:2181,192.168.33.12:2181,192.168.33.13:2181
kafaka_heap_size: 3
```

Other variables are available in [vars](vars/main.yml), usually don't need to be changed, unless we have a need to use a different mirror, for example.

> Keep in mind that the way used to set the Broker Id is by using the index of the host in the `kafka_cluster` group set in the [inventory](tests/inventory)

## Dependencies

This role doesn't have any dependencies.

## Example Playbook

A working example using Vagrant and Virtual Box is setup under [tests](./tests/).

> A [ZooKeeper Server](http://zookeeper.apache.org) is required. You can check our [zookeeper-deploy](http://github.com/ricardo-aires/zookeeper-deploy).
