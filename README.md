# Ansible : Playbook Curator

The aim of this project is to deploy Curator on Vagrant instances.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

What things you need to run this Ansible playbook :

*   [Vagrant](https://www.vagrantup.com/docs/installation/) must be installed on your computer
*   Update the Vagrant file based on your computer (CPU, memory), if needed
*   Update the operating system to deploy in the Vagrant file (default: Ubuntu)
*   If yu want to deploy Curator on Kubernetes, you must have an instance/cluster up and running and configure the Ansible host file
*   An Elasticsearch cluster should be available with some indexes to be able to run Curator commands
*   Download the Ansible requirements:

```bash
$ ansible-galaxy install -r requirements.yml
```

### Usage

A good point with Vagrant is that you can create, update and destroy all architecture easily with some commands.

Be aware that you need to be in the Vagrant directory to be able to run the commands.

#### Baremetal Deployment

To deploy the Curator client on baremetal, you have to configure the variable *curator_install_type* to *baremetal* in the file curator.yml before running the playbook :

```yaml
[...]
vars:
  curator_install_type: baremetal
[...]
```

Once it's done, you just have to provision the Vagrant instance and the Ansible playbook will automatically be called :

```bash
$ vagrant up
```

If everything run has expected, you should deploy pipeline files in /opt/curator/pipeline to manage logs.

#### Docker Deployment

To deploy the Curator client on Docker, you have to configure the variable *curator_install_type* to *docker* in the file curator.yml before running the playbook :

```yaml
[...]
vars:
  curator_install_type: docker
[...]
```

Once it's done, you just have to provision the Vagrant instance and the Ansible playbook will automatically be called :

```bash
$ vagrant up
```

If everything run has expected, you should have a Docker container named Curator which search pipeline files in the host directory : /opt/curator/pipeline

#### Kubernetes Deployment

To deploy the Curator client on Kubernetes, you have to configure the variable *curator_install_type* to *kubernetes* in the file curator.yml before running the playbook :

```yaml
[...]
vars:
  curator_install_type: kubernetes
[...]
```

Once it's done, you just have to provision the Vagrant instance and the Ansible playbook will automatically be called :

```bash
$ vagrant up
```

If everything run has expected, you should have a namespace and a pod named curator. This pod should be configured by two config map curator-config and curator-pipeline.

#### Destroy

To destroy the Vagrant resources created, just run this command :

```bash
$ vagrant destroy
```

### How-To

This section list some simple command to use and manage the playbook and the Vagrant hosts.

#### Update with Ansible

To update the Curator configuration with Ansible, you just have to run the Ansible playbook curator.yml with this command :

```bash
$ ansible-playbook curator.yml
```

#### Update with Vagrant

To update the Curator configuration with Vagrant, you just have to run provisioning part of the Vagrant file :

```bash
$ vagrant provision
```

#### Connect to Vagrant instance

To be able to connect to a Vagrant instance, you should use the CLI which is configured to automatically use the default SSH key :

```bash
$ vagrant ssh curator01
```

## Author

Member of Wikitops : https://www.wikitops.io/

## Licence

This project is licensed under the Apache License, Version 2.0. For the full text of the license, see the LICENSE file.
