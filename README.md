# postgresql

[![Source Code](https://img.shields.io/badge/github-source%20code-blue?logo=github&logoColor=white)](https://github.com/rolehippie/postgresql)
[![General Workflow](https://github.com/rolehippie/postgresql/actions/workflows/general.yml/badge.svg)](https://github.com/rolehippie/postgresql/actions/workflows/general.yml)
[![Readme Workflow](https://github.com/rolehippie/postgresql/actions/workflows/docs.yml/badge.svg)](https://github.com/rolehippie/postgresql/actions/workflows/docs.yml)
[![Galaxy Workflow](https://github.com/rolehippie/postgresql/actions/workflows/galaxy.yml/badge.svg)](https://github.com/rolehippie/postgresql/actions/workflows/galaxy.yml)
[![License: Apache-2.0](https://img.shields.io/github/license/rolehippie/postgresql)](https://github.com/rolehippie/postgresql/blob/master/LICENSE)
[![Ansible Role](https://img.shields.io/badge/role-rolehippie.postgresql-blue)](https://galaxy.ansible.com/rolehippie/postgresql)

Ansible role to install and configure a postgresql full-text search engine.

## Sponsor

Building and improving this Ansible role have been sponsored by my current and previous employers like **[Cloudpunks GmbH](https://cloudpunks.de)** and **[Proact Deutschland GmbH](https://www.proact.eu)**.

## Table of content

- [Requirements](#requirements)
- [Default Variables](#default-variables)
  - [postgresql_backup_addition_script](#postgresql_backup_addition_script)
  - [postgresql_backup_cron](#postgresql_backup_cron)
  - [postgresql_backup_enabled](#postgresql_backup_enabled)
  - [postgresql_backup_formatting](#postgresql_backup_formatting)
  - [postgresql_backup_ignore](#postgresql_backup_ignore)
  - [postgresql_backup_path](#postgresql_backup_path)
  - [postgresql_backup_retention](#postgresql_backup_retention)
  - [postgresql_ctl_options](#postgresql_ctl_options)
  - [postgresql_default_hba](#postgresql_default_hba)
  - [postgresql_exporter_args](#postgresql_exporter_args)
  - [postgresql_exporter_download](#postgresql_exporter_download)
  - [postgresql_exporter_enabled](#postgresql_exporter_enabled)
  - [postgresql_exporter_version](#postgresql_exporter_version)
  - [postgresql_extra_databases](#postgresql_extra_databases)
  - [postgresql_extra_environment](#postgresql_extra_environment)
  - [postgresql_extra_hba](#postgresql_extra_hba)
  - [postgresql_extra_ident](#postgresql_extra_ident)
  - [postgresql_extra_users](#postgresql_extra_users)
  - [postgresql_global_databases](#postgresql_global_databases)
  - [postgresql_global_environment](#postgresql_global_environment)
  - [postgresql_global_hba](#postgresql_global_hba)
  - [postgresql_global_ident](#postgresql_global_ident)
  - [postgresql_global_users](#postgresql_global_users)
  - [postgresql_listen_address](#postgresql_listen_address)
  - [postgresql_max_connections](#postgresql_max_connections)
  - [postgresql_packages](#postgresql_packages)
  - [postgresql_port](#postgresql_port)
  - [postgresql_root_password](#postgresql_root_password)
  - [postgresql_root_username](#postgresql_root_username)
  - [postgresql_ssl_ca_file](#postgresql_ssl_ca_file)
  - [postgresql_ssl_cert_file](#postgresql_ssl_cert_file)
  - [postgresql_ssl_crl_file](#postgresql_ssl_crl_file)
  - [postgresql_ssl_dh_params_file](#postgresql_ssl_dh_params_file)
  - [postgresql_ssl_enabled](#postgresql_ssl_enabled)
  - [postgresql_ssl_key_file](#postgresql_ssl_key_file)
  - [postgresql_superuser_reserved_connections](#postgresql_superuser_reserved_connections)
  - [postgresql_version](#postgresql_version)
- [Discovered Tags](#discovered-tags)
- [Dependencies](#dependencies)
- [License](#license)
- [Author](#author)

---

## Requirements

- Minimum Ansible version: `2.10`

## Default Variables

### postgresql_backup_addition_script

Additional commands at the end of the script

#### Default value

```YAML
postgresql_backup_addition_script:
```

### postgresql_backup_cron

A simple cron timing definition like hourly, daily or weekly

#### Default value

```YAML
postgresql_backup_cron: daily
```

### postgresql_backup_enabled

Enable or disable the backup script

#### Default value

```YAML
postgresql_backup_enabled: false
```

### postgresql_backup_formatting

Date format for the backup folder name

#### Default value

```YAML
postgresql_backup_formatting: '%F'
```

### postgresql_backup_ignore

Ignoring this filter via grep on database selection

#### Default value

```YAML
postgresql_backup_ignore: (postgres|template0|template1)
```

### postgresql_backup_path

Path to store the backups

#### Default value

```YAML
postgresql_backup_path: /var/lib/postgresql/backup
```

### postgresql_backup_retention

Retention period to keep backups

#### Default value

```YAML
postgresql_backup_retention: 7
```

### postgresql_ctl_options

Options for PostgeSQL ctl

#### Default value

```YAML
postgresql_ctl_options:
```

### postgresql_default_hba

List of default hba

#### Default value

```YAML
postgresql_default_hba:
  - type: local
    database: all
    user: postgres
    method: peer
  - type: local
    database: all
    user: all
    method: peer
  - type: host
    database: all
    user: all
    address: 127.0.0.1/32
    method: md5
  - type: host
    database: all
    user: all
    address: ::1/128
    method: md5
  - type: local
    database: replication
    user: all
    method: peer
  - type: host
    database: replication
    user: all
    address: 127.0.0.1/32
    method: md5
  - type: host
    database: replication
    user: all
    address: ::1/128
    method: md5
```

#### Example usage

```YAML
postgresql_default_hba:
  - type: local
    database: example
    user: postgresuser1
    address: 127.0.0.1/32
    method: md5
```

### postgresql_exporter_args

Optional list of additional arguments for the postgresql exporter

#### Default value

```YAML
postgresql_exporter_args: []
```

### postgresql_exporter_download

URL to the apache exporter to install

#### Default value

```YAML
postgresql_exporter_download: https://github.com/prometheus-community/postgres_exporter/releases/download/v{{
  postgresql_exporter_version }}/postgres_exporter-{{ postgresql_exporter_version
  }}.linux-amd64.tar.gz
```

### postgresql_exporter_enabled

Enable the installation of the apache exporter

#### Default value

```YAML
postgresql_exporter_enabled: true
```

### postgresql_exporter_version

Version of the apache exporter to install

#### Default value

```YAML
postgresql_exporter_version: 0.13.2
```

### postgresql_extra_databases

List of additional databases to create

#### Default value

```YAML
postgresql_extra_databases: []
```

#### Example usage

```YAML
postgresql_extra_databases:
  - name: example
    encoding: UTF-8
    lc_collate: de_DE.UTF-8
    lc_ctype: de_DE.UTF-8
    template: template0
    state: present
  - name: foobar
    state: absent
```

### postgresql_extra_environment

Dict of extra environment variables

#### Default value

```YAML
postgresql_extra_environment: {}
```

#### Example usage

```YAML
postgresql_extra_environment:
  FOO: bar
  EXAMPLE: works
```

### postgresql_extra_hba

List of extra hba

#### Default value

```YAML
postgresql_extra_hba: []
```

#### Example usage

```YAML
postgresql_extra_hba:
  - type: local
    database: example
    user: postgresuser1
    address: 127.0.0.1/32
    method: md5
```

### postgresql_extra_ident

List of extra identities

#### Default value

```YAML
postgresql_extra_ident: []
```

#### Example usage

```YAML
postgresql_extra_ident:
  - map: example1
    system: systemuser1
    postgres: postgresuser1
  - map: example2
    system: systemuser2
    postgres: postgresuser2
```

### postgresql_extra_users

List of additional users to create

#### Default value

```YAML
postgresql_extra_users: []
```

#### Example usage

```YAML
postgresql_extra_users:
  - name: example
    password: p433w0rd
    db: test
    groups:
      - group1
      - group2
    priv: ALL/example:ALL
    role_attr_flags: CREATEDB,CREATEROLE,SUPERUSER
    state: present
  - name: foobar
    state: absent
```

### postgresql_global_databases

List of databases to create

#### Default value

```YAML
postgresql_global_databases: []
```

#### Example usage

```YAML
postgresql_global_databases:
  - name: example
    encoding: UTF-8
    lc_collate: de_DE.UTF-8
    lc_ctype: de_DE.UTF-8
    template: template0
    state: present
  - name: foobar
    state: absent
```

### postgresql_global_environment

Dict of global environment variables

#### Default value

```YAML
postgresql_global_environment: {}
```

#### Example usage

```YAML
postgresql_global_environment:
  FOO: bar
  EXAMPLE: works
```

### postgresql_global_hba

List of global hba

#### Default value

```YAML
postgresql_global_hba: []
```

#### Example usage

```YAML
postgresql_global_hba:
  - type: local
    database: example
    user: postgresuser1
    address: 127.0.0.1/32
    method: md5
```

### postgresql_global_ident

List of global identities

#### Default value

```YAML
postgresql_global_ident: []
```

#### Example usage

```YAML
postgresql_global_ident:
  - map: example1
    system: systemuser1
    postgres: postgresuser1
  - map: example2
    system: systemuser2
    postgres: postgresuser2
```

### postgresql_global_users

List of users to create

#### Default value

```YAML
postgresql_global_users: []
```

#### Example usage

```YAML
postgresql_global_users:
  - name: example
    password: p433w0rd
    db: test
    groups:
      - group1
      - group2
    priv: ALL/example:ALL
    role_attr_flags: CREATEDB,CREATEROLE,SUPERUSER
    state: present
  - name: foobar
    state: absent
```

### postgresql_listen_address

Address to listen to

#### Default value

```YAML
postgresql_listen_address: 0.0.0.0
```

### postgresql_max_connections

Maximum of allowed connections

#### Default value

```YAML
postgresql_max_connections: 1000
```

### postgresql_packages

List of packages to install

#### Default value

```YAML
postgresql_packages:
  - postgresql-{{ postgresql_version }}
  - postgresql-client-{{ postgresql_version }}
  - python3-psycopg2
```

### postgresql_port

Port to listen to

#### Default value

```YAML
postgresql_port: 5432
```

### postgresql_root_password

Password for the root user

#### Default value

```YAML
postgresql_root_password:
```

### postgresql_root_username

Username for the root user

#### Default value

```YAML
postgresql_root_username: postgres
```

### postgresql_ssl_ca_file

#### Default value

```YAML
postgresql_ssl_ca_file:
```

### postgresql_ssl_cert_file

#### Default value

```YAML
postgresql_ssl_cert_file: /etc/ssl/certs/ssl-cert-snakeoil.pem
```

### postgresql_ssl_crl_file

#### Default value

```YAML
postgresql_ssl_crl_file:
```

### postgresql_ssl_dh_params_file

#### Default value

```YAML
postgresql_ssl_dh_params_file:
```

### postgresql_ssl_enabled

Enable SSL encryption

#### Default value

```YAML
postgresql_ssl_enabled: true
```

### postgresql_ssl_key_file

#### Default value

```YAML
postgresql_ssl_key_file: /etc/ssl/private/ssl-cert-snakeoil.key
```

### postgresql_superuser_reserved_connections

Reserved connections for superuser

#### Default value

```YAML
postgresql_superuser_reserved_connections: 3
```

### postgresql_version

#### Default value

```YAML
postgresql_version: "{{ '14' if ansible_distribution_version is version('20.04', '>')
  else ('12' if ansible_distribution_version is version('18.04', '>') else '10') }}"
```

## Discovered Tags

**_postgresql_**

**_postgresql-exporter_**


## Dependencies

- None

## License

Apache-2.0

## Author

[Thomas Boerger](https://github.com/tboerger)
