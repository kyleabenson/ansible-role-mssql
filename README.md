Role Name
=========

This role will perform install and removal of the Microsoft SQL Server and command line tools for RHEL 7.

Future functionality will offer a quick way to create a new DB directly or load in an existing dump.

Requirements
------------

In order for this role to work, you need some core repositories configured for your RHEL instance. If running this in a public cloud provider, this has likely already been done for you. If necessary, register the system to Red Hat's content repositories or Red Hat Satellite using `subscription-manager`.




Role Variables
--------------

The only variables in Defaults are around the mssql packages and should not need to be changed. Pip is also included in order to handle the expect module for accepting the EULA.

Within Vars, you must explicitly agree to the End User's License Agreement for both the server setup script and the command line tools. To do this, add Y or YES where applicable to the variables for each EULA.

The default user is 'SA' when logging in via command line tools

Additionally, there are some predefined default values including:
```yaml
database_password: 'P@ssWORD!' # This sets the database password
edition: Developer # This is default edition and what the EULA covers
upgrade: false # Setting this to true will ensure that the installation removes older devel packages first
enable_iptables: false #Setting this to true will enable iptables rules
```

Dependencies
------------

No additional galaxy roles are required.

Example Playbook
----------------

To use the default installation tasks:

    - hosts: db
      roles:
         - { role: kyle-benson.mssql-server }

To use the removal tasks:

    - hosts: db
      name: Removes mssql-server
      become: yes

      tasks:
      - name: Run remove tasks from mssql-server role
        include_role:
          name: mssql-server
          tasks_from: remove

License
-------

BSD

Author Information
------------------

Contributions and issues with this role are welcome at the associated git repo.
