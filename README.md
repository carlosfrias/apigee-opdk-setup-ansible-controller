Apigee OPDK Setup Ansible Controller
=========

This role will setup an Ansible controller with common structure. 

Requirements
------------

* This role uses properties to generate a basic ansible.cfg. 
* The remote user must be provided.  
* Templates for credentials.yml and customer-properties.yml are provided as a starting
point. 

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

| Variable | Description |
| --- | --- |
| ansible_workspace | This variable indicates the path where Ansible working files should be installed. The variable defaults to the current playbook directory. |
| apigee_config_folder | This variable indicates the path to the apigee configuration files. This variable default to the current playbook directory. |
| remote_user | This variable indicates the remote user that Ansible will be configured to use in the ansible.cfg. There is no default possible.  | 

Dependencies
------------

NA

Example Playbook
----------------

This is an example of how to invoke this role: 

    - name: Setup the controller workspace
      hosts: localhost
      connection: local
      gather_facts: no
    
      vars:
        remote_user: friasc
    
      roles:
      - { role: apigee-opdk-setup-ansible }

License
-------

Apache 2.0

Author Information
------------------

Carlos Frias

