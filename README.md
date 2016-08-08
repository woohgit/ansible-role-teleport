# Ansible Role: Teleport

[![Build Status](https://travis-ci.org/woohgit/ansible-role-teleport.svg?branch=master)](https://travis-ci.org/woohgit/ansible-role-teleport)

An Ansible Role that installs Teleport on RHEL/CentOS, Debian/Ubuntu, SLES.

## Requirements

You will need to provide your own SSL certificate and key files. You can generate a self-signed certificate with a command like `openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout example.key -out example.crt`. If no SSL certs/keys are given, teleport will automatically generate one for you.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    teleport_version: "1.0.4"
    teleport_ssl_cert_path: "/etc/teleport"
    teleport_config_path: "/etc/teleport.yaml"
    teleport_nodename: "teleport"
    teleport_auth_servers:
        - 127.0.0.1:3025
    teleport_data_dir: "/var/lib/teleport"

Teleport stores the data locally under the `teleport_data_dir`.

    teleport_log_level: 'WARN'
    teleport_storage_type: 'bold'
    teleport_pidfile: '/var/run/teleport.pid'

    teleport_auth_enabled: true
    teleport_auth_listen_address: '0.0.0.0:3025'
    teleport_auth_cluster_name: 'main'
    teleport_auth_tokens: []
    teleport_auth_trusted_clusters: []
    teleport_auth_oidc_connectors: []


    teleport_ssh_enabled: true

If you don't want to login to this server using Teleport, only via the standard SSH way, disable the SSH service by setting this value to `false`.
    
    teleport_ssh_listen_address: '0.0.0.0:3022'
    teleport_commands: []

    teleport_proxy_enabled: true

If you want to disable the WebUI (proxy), set this setting to `false`.

    teleport_proxy_listen_address: '0.0.0.0:3023'
    teleport_proxy_web_listen_address: '0.0.0.0:3080'
    teleport_proxy_tunnel_listen_address: '0.0.0.0:3024'
    teleport_proxy_https_key_file: ''
    teleport_proxy_https_cert_file: ''

## Dependencies

None.

## Example Playbook

    - hosts: teleport_proxies
      vars_files:
        - vars/main.yml
      roles:
        - { role: woohgit.teleport }

*Inside `vars/main.yml`*:

    teleport_ssh_enabled: false

## License

MIT / BSD

## Author Information

This role was created in 2016 by [Adam Papai](http://www.wooh.hu/).