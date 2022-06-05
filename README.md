<p><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4e/Docker_%28container_engine%29_logo.svg/2560px-Docker_%28container_engine%29_logo.svg.png" alt="docker-logo" title="docker" align="top" height=100 /></p>

***Docker** is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker’s methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.*

### General informations

This Ansible role is designed to deploy and manage Docker Engine on target servers. This role only deploy the Community Edition of Docker.

**Table of Contents**

  - [Roles variables](#role-variables)
  - [Examples](#examples)
  - [Install and use this role](#install-and-use-this-role)

**Supported Platforms**

  - Ubuntu 22.04

**References**

  - Docker Engine : https://docs.docker.com/engine/
  - Docker Compose : https://docs.docker.com/compose/

**Implementation notes**

  - ***Socket Activation*** : this role aims server deployments of Docker, therefore daemon socket activation is disabled (to avoid breaking of containers auto start on boot)
  - ***Docker Compose*** : this role can also install Docker Compose, but only the v2 version as plugin. The Docker Compose v1 (standalone `docker-compose` CLI) is now deprecated by Docker developers.

### Role variables

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `docker_version`                  | `20.10.16`                   | Defines the version of Docker Engine to install                  |
| `docker_daemon_configuration`     | `{}`                         | YAML dict containing the Docker daemon configuration             |
| `docker_compose_plugin_enabled`   | `false`                      | If set to `true`, install the Docker Compose v2 plugin           |
| `docker_compose_plugin_version`   | `2.5.0`                      | Defines the version of the Docker Compose v2 plugin to install   |

### Examples

* Configure the Docker daemon 

  ```YAML
  docker_daemon_configuration:
    userland-proxy: false
    iptables: true
    live-restore: true
    features:
      buildkit: true
  ```

* Install Docker Compose plugin (Docker Compose v2)

  ```YAML
  docker_compose_plugin_enabled: true

  # You can also specify the version
  docker_compose_plugin_version: "2.5.0"
  ```

### Install and use this role

* Install the role using the command-line :

  ```shell
  $ ansible-galaxy role install git+https://github.com/ruskofd/docker-role.git
  ```

* Install the collection in your projects using a `requirements.yml` file and `ansible-galaxy` command-line :

  ```YAML
  $ cat requirements.yml
  ---
  roles:
    - name: docker
      src: https://github.com/ruskofd/docker-role.git
      scm: git
      version: '1.0.0'

  $ ansible-galaxy install-f -r requirements.yml
  ```

* Once the collection is installed, you can use the roles in your playbooks :

  ```yaml
  - name: Install Docker
    hosts: docker
    roles:
      - role: docker
  ```
