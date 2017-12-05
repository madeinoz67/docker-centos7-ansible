# CentOS 7.x Ansible CI Test Image

[![Docker Automated build](https://img.shields.io/docker/automated/shireofdardanup/centos7-ci.svg?maxAge=2592000)](https://hub.docker.com/r/shireofdardanup/centos7-ci/)

CentOS 7.x Docker container for Ansible playbook, role and Prometheus configuration testing.  Ansible-lint is also installed to check ansible playbook files for best practice.

## How to Build

This image is built on Docker Hub automatically any time the upstream OS container is rebuilt, and any time a commit is made or merged to the `master` branch. But if you need to build the image on your own locally, do the following:

  1. [Install Docker](https://docs.docker.com/engine/installation/).
  2. `cd` into this directory.
  3. Run `docker build -t centos7-ci .`

## How to Use

  1. [Install Docker](https://docs.docker.com/engine/installation/).
  2. Pull this image from Docker Hub: `docker pull shireofdardanup/centos7-ci:latest` (or use the tag you built earlier, e.g. `centos7-ci`).
  3. Run a container from the image: `docker run --detach --privileged --volume=/sys/fs/cgroup:/sys/fs/cgroup:ro shireofdardanup/centos7-ci:latest /usr/lib/systemd/systemd` (to test my Ansible roles, I add in a volume mounted from the current working directory with ``--volume=`pwd`:/etc/ansible/roles/role_under_test:ro``).
  4. Use Ansible inside the container:
    a. `docker exec --tty [container_id] env TERM=xterm ansible --version`
    b. `docker exec --tty [container_id] env TERM=xterm ansible-playbook /path/to/ansible/playbook.yml --syntax-check`
  5. Use Promtool inside the container: [Prometheus Alert Rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)
    a. `docker exec --tty [container_id] env TERM=xterm /prometheus/promtool --version`
    b. `docker exec --tty [container_id] env TERM=xterm /prometheus/promtool check rules /path/to/alert.rule`
  6. Use ansible-lint inside the container
    a. `docker exec --tty [container_id] env TERM=xterm ansible-lint --version`
    b. `docker exec --tty [container_id] env TERM=xterm ansible-lint /path/to/playbook.yml`

## Notes

I use Docker to test my Ansible roles and playbooks on multiple OSes using CI tools like Jenkins,Travis and Gitlab. This container allows me to test roles and playbooks using Ansible running locally inside the container.

This container is also used to test [Prometheus](https://prometheus.io) configurations using promtool

[Prometheus Alert Rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)

> **Important Note**: I use this image for testing in an isolated environment—not for production—and the settings and configuration used may not be suitable for a secure and performant production environment. Use on production servers/in the wild at your own risk!

## Author

Prometheus Promtool and ansible-lint added in 2017 by Stephen Eaton seaton@dardanup.wa.gov.au

Original Image Created in 2016 by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
