# Ansible : Playbook Curator

The purpose of this project is to deploy a simple Curator client on Vagrant with some default value. The deployment can be baremetal, on Docker or on Kubernetes / Openshift.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

What things you need to run this Ansible playbook :

*   [Vagrant](https://www.vagrantup.com/docs/installation/) must be installed on your computer
*   Update the Vagrant file based on your computer (CPU, memory), if needed
*   You must have download the ubuntu Xenial64 vagrant box :

```bash
$ vagrant box add https://app.vagrantup.com/ubuntu/boxes/xenial64
```
*   If yu want to deploy Curator on Kubernetes, you must have an instance/cluster up and running and configure the Ansible host file
*   An Elasticsearch cluster should be available with some indexes to be able to run Curator commands

### Usage

A good point with Vagrant is that you can create, update and destroy all architecture easily with some commands.

Be aware that you need to be in the Vagrant directory to be able to run the commands.

#### Build Environment

This section does not have to be played if you want to deploy Curator on Kubernetes.

Vagrant needs to init the project to run and build it :

```bash
$ vagrant up
```

After build, you can check which virtual machine Vagrant has created :

```bash
$ vagrant status
```

If all run like it is expected, you should see something like this :

```bash
$ vagrant status

Current machine states:

curator01                  running (virtualbox)
```

#### Baremetal Deployment

This playbook has some dependencies to other roles that must be downloaded before executing the playbook :

```bash
$ ansible-galaxy install -r requirements.yml
```

This command should download the Pip role from Ansible Galaxy to the local role path.

To deploy the Curator client, you just have to run the Ansible playbook curator.yml with this command :

```bash
$ ansible-playbook curator.yml
```

If everything run has expected, you should deploy pipeline files in /opt/curator/pipeline to manage logs.

#### Docker Deployment

This playbook has some dependencies to other roles that must be downloaded before executing the playbook :

```bash
$ ansible-galaxy install -r requirements.yml
```

This command should download the Docker and pip roles from Ansible Galaxy to the local role path.

To deploy the Curator client on Docker, you have to configure the variable *curator_on_docker* to *true* in the file curator.yml before running the playbook :

```yaml
[...]
vars:
  curator_on_baremetal: false
  curator_on_docker: true
  curator_on_kubernetes: false
[...]
```

Once it's done, you just have to run the Ansible playbook curator.yml with this command :

```bash
$ ansible-playbook curator.yml
```

If everything run has expected, you should have a Docker container named Curator which search pipeline files in the host directory : /opt/curator/pipeline

#### Kubernetes Deployment

To deploy the Curator client on Docker, you have to configure the variable *curator_on_kubernetes* to *true* in the file curator.yml before running the playbook :

```yaml
[...]
vars:
  curator_on_baremetal: false
  curator_on_docker: false
  curator_on_kubernetes: true
[...]
```

Once it's done, you just have to run the Ansible playbook curator.yml with this command :

```bash
$ ansible-playbook curator.yml
```

If everything run has expected, you should have a namespace and a pod named curator. This pod should be configured by two config map curator-config and curator-pipeline.

#### Destroy

To destroy on what Vagrant has created, just run this command :

```bash
$ vagrant destroy
```

## Author

Member of Wikitops : https://www.wikitops.io/

## Licence

This project is licensed under the Apache License, Version 2.0. For the full text of the license, see the LICENSE file.
