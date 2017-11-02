# Ansible Role: Teleport

[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-woohgit.teleport-blue.svg)](https://galaxy.ansible.com/woohgit/teleport/) [![Build Status](https://travis-ci.org/woohgit/ansible-role-teleport.svg?branch=master)](https://travis-ci.org/woohgit/ansible-role-teleport)

An Ansible Role that installs [Teleport](https://gravitational.com/teleport/) on RHEL/CentOS, Debian/Ubuntu, SUSE.

Teleport is an SSH for Clusters and Teams


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
    teleport_storage_type: 'bolt'
    teleport_pidfile: '/var/run/teleport.pid'

    teleport_auth_enabled: true
    teleport_auth_listen_address: '0.0.0.0:3025'
    teleport_auth_cluster_name: 'main'


    teleport_auth_tokens_node: []
    teleport_auth_tokens_proxy: []
    teleport_auth_tokens_auth: []


You probably want to have multiple nodes joined to our cluster. You can do that with temporary tokens or you can automate the process and use static tokens. The 3 well known roles - auth, proxy, node - can have 3 different tokens.

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

For full reference see the official [teleport documentation by gravitational](http://gravitational.com/teleport/docs/quickstart/).

## Dependencies

None.

## Core Concepts

There are three types of services (roles) in a Teleport cluster.

- Proxy service accepts inbound connections from the clients and routes them to the appropriate nodes. The proxy also serves the Web UI.
- Auth service provides authentication and authorization service to proxies and nodes. It is the certificate authority (CA) of a cluster and the storage for audit logs. It is the only stateful component of a Teleport cluster.
- Node role provides the SSH access to a node. Typically every machine in a cluster runs teleport with this role. It is stateless and lightweight.

For more details about teleport architecture, please refer to the [official documentation](http://gravitational.com/teleport/docs/architecture/#architecture).

## Example Playbook for setting up a Teleport proxy and auth server without node role.


    - hosts: teleport_proxies
      vars_files:
        - vars/main.yml
      roles:
        - { role: woohgit.teleport }


*Inside `vars/main.yml`*

    teleport_ssh_enabled: false
    teleport_auth_tokens_node:
      - xxxx-yyyy-xxxx

If you want to be able to login to the proxy host too using teleport, set `teleport_ssh_enabled` to `true`.


## Example Playbook for setting up a Teleport node.

You can automatically connect a node to the proxy server by providing same same auth_token.

    - hosts: teleport_nodes
      vars_files:
        - vars/main.yml
      roles:
        - { role: woohgit.teleport }


*Inside `vars/main.yml`*:

    teleport_ssh_enabled: true
    teleport_auth_enabled: false
    teleport_proxy_enabled: false
    teleport_auth_servers:
      - ip_of_the_proxy_server
    teleport_auth_token: xxxx-yyyy-xxxx


## License

MIT / BSD

## Author Information

This role was created in 2016 by [Adam Papai](http://www.wooh.hu/).