logstash-forwarder
=========

This role will install logstash-forwarder using apt/rpm packages hosted in your apt/yum repositories

Requirements
------------

This role utilizes apt and/or yum to manage packages so the appropriate package manager is required.

Role Variables
--------------
| variable | description | default | mandatory
|----------|-------------|---------|----------
| `logstash_forwarder_state` | present or absent | present |
| `logstash_forwarder_enabled` | start on boot| yes |
| `logstash_forwarder_start` | start after installing (ensure running) | false |
| `logstash_forwarder_config_dir` | configuration directory. will be created if it absent| /etc/logstash-forwarder/conf.d |
| `logstash_forwarder_split_files` | should conf files be created separately in .d fashion (not supported by all versions) | true |
| `logstash_forwarder_log_to_syslog` | log to syslog vs stdout | true |
| `logstash_forwarder_cpuprofile` | cpuprofile option for logstash-forwarder |  | no
| `logstash_forwarder_spool_size` | spool size to keep in memory before flush | 1024 |
| `logstash_forwarder_idle_flush_time` | time (in sec) before a flush |  | no
| `logstash_forwarder_from_beginning` | start reading from beginning of files on every start | false |
| `logstash_forwarder_servers` | a list of servers to connect and send messages to |  | yes
| `logstash_forwarder_ssl_cert` | path to client ssl certificate |  | no
| `logstash_forwarder_ssl_key` | path to client ssl key |  | no
| `logstash_forwarder_ssl_ca` | path to trusted ssl certificate authority file |  | yes
| `logstash_forwarder_files` | array of hashes, each with paths and fields for associated files |  | no
| `logstash_forwarder_files.<app_name>.fields` | hash of fields to add to events.  Ex. tags: test |  | no
| `logstash_forwarder_files.<app_name>.paths` | list of paths to forward logs from.  single files or globs (*.json) is acceptable |  | no

Dependencies
------------

No dependencies.

Example Playbook
----------------

This is typical usage for applications.  They only define the files (with paths, fields, etc) that they want forwarded.

    - hosts: all
     roles:
       - role: logstash-forwarder
         logstash_forwarder_files:
           test_app:
             fields:
               tags: test
             paths:
               - /var/log/test
               - /somewhere/else/test
           another_app:
             fields:
               tags: app2
             paths:
               - /var/log/another_app

This is typical usage for things like underlying AMIs or base installs.

    - hosts: all
      roles:
        - role: logstash-forwarder
          logstash_forwarder_state: present
          logstash_forwarder_start: true
          logstash_forwarder_enabled: true
          logstash_forwarder_version: 0.3.1.20150205.19
          logstash_forwarder_split_files: true
          logstash_forwarder_servers:
            - server1
            - server2
          logstash_forwarder_ssl_cert: sslcert
          logstash_forwarder_ssl_key: sslkey
          logstash_forwarder_ssl_ca: sslca

Here is a rundown on typical usage that does everything: installs, configures, and starts.

    - hosts: all
      roles:
        - role: logstash-forwarder
          logstash_forwarder_state: present
          logstash_forwarder_start: true
          logstash_forwarder_version: 0.3.1.20150205.19
          logstash_forwarder_split_files: true
          logstash_forwarder_servers:
            - server1
            - server2
          logstash_forwarder_ssl_cert: sslcert
          logstash_forwarder_ssl_key: sslkey
          logstash_forwarder_ssl_ca: sslca
          logstash_forwarder_files:
            test_app:
              fields:
                tags: test
              paths:
                - /var/log/test
                - /somewhere/else/test
            another_app:
              fields:
                tags: app2
              paths:
                - /var/log/another_app

License
-------

Apache 2.0

Author Information
------------------

Author: Ryan O'Keeffe (okeefferd@gmail.com)
