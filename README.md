Ansible Role: splunk-forwarder
=========
[![Molecule](https://github.com/troyfontaine/ansible-role-splunk-forwarder/workflows/Molecule/badge.svg?event=push)](https://github.com/troyfontaine/ansible-role-splunk-forwarder/actions?query=workflow%3AMolecule)
![Latest Version](https://img.shields.io/github/v/tag/troyfontaine/ansible-role-splunk-forwarder?sort=semver&label=Latest%20Version)
[![License](https://img.shields.io/github/license/troyfontaine/ansible-role-splunk-forwarder)](https://github.com/troyfontaine/ansible-role-splunk-forwarder/blob/master/LICENSE)

This role will deploy the Splunk universal forwarder.

Requirements
------------

This role is tested on Ubuntu 16.04, 18.04, 20.04, 22.04, Oracle Linux 8 and Amazon Linux 2, but should probably work on any systemd based system.


Role Variables
--------------

### Default

For most people, the default variables that are set should be fine, but there are use cases for changing them.  They are:


     splunk_forwarder_user                      # Default User (splunkfwd)
     splunk_forwarder_group                     # Default Group (splunkfwd)
     splunk_forwarder_uid                       # Default UID (10011)
     splunk_forwarder_gid                       # Default GID (10011)
     splunk_release                             # Default Release Version (9.1.0.1)
     splunk_url                                 # Default Download URL              


### Playbook Variables

Within your playbook, you should set the following variables:

    splunk_forwarder_admin_user:            # Set the administrative user for the forwarder
    splunk_forwarder_admin_pass:            # Set the administrative password for the forwarder
    splunk_forwarder_depl_server:           # Set to the URL:Port of your splunk deployment server i.e. "splunk-mgt:8089" (optional)
    splunk_forwarder_indexer_hostname:      # Set to a list of the hostnames of your splunk indexer(s) i.e. "splunk-indexer"
    splunk_forwarder_output_use_tls:        # Set to true then adjust your variables as necessary
    splunk_forwarder_default_index:         # Set to the index that the forwarder should use i.e. "default"
    splunk_forwarder_default_sourcetype:    # Set the Source type i.e. "nginx"

You also need to set what logs to forward. You can do so using a list:

    splunk_forwarder_logs:
      - { 'path': '/var/log/nginx/access.log', 'sourcetype': 'nginx', 'index': 'nginx' }
      - { 'path': '/var/log/nginx/error.log', 'sourcetype': 'nginx', 'index': 'nginx' }

Dependencies
------------

You must have a splunk indexer running in your environment.

Example Playbook
----------------

You should define the required variables in your playbook and call the role:

    - hosts: nginx
      remote_user: ec2-user
      become: True
      vars:
        splunk_forwarder_indexer_hostname:
          - "splunk-indexer:9997"
        splunk_forwarder_default_index: "prodapps"
        splunk_forwarder_default_sourcetype: "nginx"
        splunk_forwarder_logs = [
          {"path": "/var/log/nginx/access.log", "sourcetype": "nginx", "index": "nginx"},
          {"path": "/var/log/nginx/error.log", "sourcetype": "nginx", "index": "nginx"}
        ]
        roles:
          - splunk-forwarder

If you want to run this against an AmazonLinux instances, add the following to your playbook, otherwise it will fail.:

     pre_tasks:
       - set_fact: ansible_distribution_major_version=6
         when: ansible_distribution == "Amazon" and ansible_distribution_major_version == "NA"


License
-------

MIT


Author Information
------------------

Mark Honomichl aka [AustinCloudGuru](https://austincloud.guru)
Created in 2016 
