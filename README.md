# app_discovery_deploy

## Description
Deploy app discovery backend using ansible

## Prerequisite
- VirtualBox >=4.3.30
- Vagrant >=1.7.4
- Following Vagrant plugs:

        vagrant plugin install vagrant-hosts vagrant-hostmanager

- Ansible >=1.9.2

## Initial Setup

    vagrant up

## API Server Local Development
- Change playbook to `api_server.yml` in `Vagrantfile`
- Build new jar
- Run `vagrant provision`
