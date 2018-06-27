Apigee OPDK Setup Ansible Controller
=========

This role will setup an Ansible controller with common structure. 

Requirements
------------

* This role uses properties to generate a basic ansible.cfg; defaults are provided. 
* The `remote_user` variable must be provided.  
* Templates for credentials.yml and customer-properties.yml are provided as a starting
point. 

Role Variables
--------------

A description of the variables for this role can be found below: 

| Variable | Description |
| --- | --- |
| remote_user | This variable indicates the remote user that Ansible will be configured to use in the ansible.cfg. There is no default possible.  | 
| ansible_workspace | This variable indicates the path where Ansible working files should be installed. The variable defaults to the current playbook directory. |
| apigee_config_folder | This variable indicates the path to the apigee configuration files. This variable default to the current playbook directory. |
| ansible_config | This variable indicates the location of the generated ansible.cfg |
| local_workspace | This variable populates the default value if `ansible_workspace` is not provided |
| setup_dirs | This collection provides the standard list of folders created in the `ansible_workspace` |
| target_apigee_config_folder | This variable populates the default value if the `apigee_config_folder` is not provided. |
| starter_templates | This collections provides the list of template files that will be placed under the `apigee_config_folder` |
| inventory_folder | This variables indicates the location of the inventory folder. |
| library_dir | This variable contains the location of the Ansible library folder. |
| pip_index_url | This variable contains the url to the PIP server that PIP should used. A `pip.conf` file will be generated if this is provided, otherwise no file will be generated|

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

