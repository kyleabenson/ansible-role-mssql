Role Name
=========

This role will perform the following:
- Install and uninstall of the Microsoft SQL Server
- Create, delete, or import (from .sql file) a database
- Optional command line tools for RHEL 7

Requirements
------------

In order for this role to work, you need some core repositories configured for your RHEL instance. If running this in a public cloud provider, this has likely already been done for you. If necessary, register the system to Red Hat's content repositories or Red Hat Satellite using `subscription-manager`.




Role Variables
--------------

The only variables in Defaults are around the mssql packages and should not need to be changed. Pip is also included in order to handle the expect module for accepting the EULA.

Within Vars, you must explicitly agree to the End User's License Agreement for both the server setup script and the command line tools. To do this, add Y or YES where applicable to the variables for each EULA.

The default user is 'SA' when logging in via command line tools. The SA user is mandatory for initial creation, this role does not currently offer the ability to create additional users.

Additionally, there are some predefined default values including:
```yaml
end_user_license_aggreement_consent_server: # Must be Y or N
end_user_license_aggreement_consent_cli: "" # Must be YES or NO in all caps within quotes
database_password: 'P@ssWORD!'
edition: Developer

# For use when creating, importing, or deleting databases
db_name:
db_host: 127.0.0.1
db_port: 1433
db_user: SA

import: false
import_file:
import_file_dest:

#System Config options
enable_iptables: false
install_cli: false

```

Dependencies
------------

No additional galaxy roles are required.

Example Playbook
----------------

To use the default installation tasks:

    - hosts: db
      roles:
         - { role: kyleabenson.mssql }

To use the installation and create a new db:
```yaml
    - hosts: db
      roles:
        - { role: kyleabenson.mssql }
      tasks:
        - name: Create new db
          include_role:
            name: kyleabenson.mssql
            tasks_from: new_db
```
To use the uninstall tasks:
```yaml
    - hosts: db
      name: Removes mssql-server
      become: yes

      tasks:
      - name: Run remove tasks from mssql-server role
        include_role:
          name: kyleabenson.mssql
          tasks_from: uninstall
```

License
-------

BSD

Author Information
------------------

Contributions and issues with this role are welcome at the associated git repo.
