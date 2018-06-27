Apigee OPDK Setup Ansible Controller
=========

This role will setup an Ansible controller with common structure. The default location for this is 
indicated by `ansible_workspace` and `apigee_workspace`. `ansible_workspace` and `apigee_workspace`
default to `~/apigee-workspace` as indicated in the Role Variables below. This role will produce the
following structure: 

    apigee-workspace/
        ├── ansible.cfg
        ├── apigee
        │   └── custom-properties.yml
        ├── apigee-secure
        │   └── credentials.yml
        ├── configurations
        │   ├── aio.cfg
        │   └── templates
        ├── inventory
        │   ├── aio
        │   └── templates
        ├── library
        │   └── library
        ├── logs
        ├── playbooks
        ├── roles
        └── tmp


Requirements
------------

* This role uses properties to generate a basic ansible.cfg; defaults are provided. 
* The `remote_user` variable must be provided.  
* Templates for credentials.yml and customer-properties.yml are provided as a starting point. 
* git and rsync must be installed.

Role Variables
--------------

A description of the variables for this role can be found below: 

| Variable | Description |
| --- | --- |
| remote_user | This variable indicates the remote user that Ansible will be configured to use in the ansible.cfg. There is no default possible for the `remote_user`.  | 
| ansible_workspace | This variable indicates the path where Ansible working files should be installed. The variable defaults to `~/apigee-workspace`. |
| apigee_workspace | This variable indicates the path to the apigee configuration files. The variable defaults to '~/apigee-workspace`. |
| ansible_config | This variable indicates the location of the generated ansible.cfg |
| local_workspace | This variable populates the default value if `ansible_workspace` is not provided |
| setup_dirs | This collection provides the standard list of folders created in the `ansible_workspace` |
| target_apigee_config_folder | This variable populates the default value if the `apigee_config_folder` is not provided. |
| starter_templates | This collections provides the list of template files that will be placed under the `apigee_config_folder` |
| inventory_folder | This variables indicates the location of the inventory folder. |
| library_dir | This variable contains the location of the Ansible library folder. |
| pip_index_url | This variable contains the url to the PIP server that PIP should used. A `pip.conf` file will be generated if this is provided, otherwise no file will be generated |
| pip_config | This variable contains the final name of the pip file and the location. |
| private_key_file | This variable contains the path to the SSH private key Ansible will use to connect |
| repository_secure_endpoint_https | This variable contains the URL to the git https repository endpoint. |
| repository_secure_endpoint_ssh | This variable contains the URL to the git ssh repository endpoint. |
| checkout_type | This is used to select which repository endpoint should be used during a checkout. Possible values are 'https' or 'ssh'. The default value is 'https' |
| template_repos | This collection contains a listing of the github repos to checkout. The structure of each element is: `- { dir: "{{ target_ansible_workspace }}/inventory", repo_name: apigee-opdk-ansible-inventory-samples, dest_name: templates }` |
 

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

<!-- BEGIN Google How To Contribute -->
# How to Contribute

We'd love to accept your patches and contributions to this project. Please review our [guidelines](CONTRIBUTING.md).
<!-- END Google How To Contribute -->
<!-- BEGIN Google Required Disclaimer -->

# Not Google Product Clause

This is not an officially supported Google product.
<!-- END Google Required Disclaimer -->
