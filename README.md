# Ansible playbook for winepak

This repo contains the Ansible playbook to setup, deploy, and maintain the winepak infrastructure. This playbook is designed to be Flatpak repo agnostic. You should be able top copy this repo, change the hosts in `hosts`, change the general settings in `group_vars/*`, change the host specific settings in `host_vars/`, and create a similar environment as winepak's setup. The only exception being the buildbot-config.

The playbook will:

- Setup a base environment on Fedora or Debian (un-tested)
  - Create admin users
  - Add SSH keys
  - Configure passwordless sudo
  - Install base packages
  - Configure timezone to UTC
  - Configure fail2bain
- Setup frontend
  - Install a NGINX server
  - Configure server, ports, and firewall
  - Setup list of domains in `[frontend]` group
  - Setup SSL for domains via Let's Encrypt (certbot)
  - Setup auto-renew for SSL
  - Setup website
  - Setup download repo
  - Setup Buildbot proxy
- Setup Buildbot
  - Create buildbot user & group
  - Install Buildbot in virtualenv for domains in `[buildbot]` group
  - Configure service, ports, & firewall
  - Install Buildbot web frontend
  - Setup buildbot-master
  - Setup buildbot config.json
  - Setup buildbot workers.json
- Setup buildbot-worker(s)
  - Install buildbot-worker into virtualenv for domains in `[buildbot-worker]` group
  - Configure service, ports, & firewall
  - Setup authentication and connecting to buildbot-master

Since each Buildbot config is different you'll need to set that up on your own. Buildbot and it's configs are installed into `/srv/buildbot/` for both buildbot masters & workers.

## Run
```
ansible-playbook playbook.yml
```
