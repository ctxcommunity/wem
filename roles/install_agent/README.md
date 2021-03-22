Role Name
=========

Role Name: ctxcommunity.wem.install_agent

This role is used to Install the Citrix Workspace Management Agent from an SMB share.

Requirements
------------

The Microsoft .NET 4.8 Framework is required for most versions.

Role Variables
--------------

The defaults/main.yml contains the following variables and default values:

  wem_agent_versions:
    2006:
      install_arguments: /quiet Cloud=0
      product_id: '{3E82149F-130C-49AB-B067-539851AE66E3}'
    2009:
      install_arguments: /quiet Cloud=0
      product_id: '{C14EE685-0538-4778-9B52-AD4C8B934040}'
    2012:
      install_arguments: /quiet Cloud=0
      product_id: '{4A001FDB-9C71-48B3-8A39-311F1317C042}'
    2103:
      install_arguments: /quiet Cloud=0
      product_id: '{FBF14783-5F94-4B02-ACA7-063AE03991D9}'

  smb_location: \\file1\automation

  wem_agent_version: 2103
  wem_agent_smb_location: "{{ smb_location }}\\Citrix\\Workspace Environment Management\\{{ wem_agent_version }}"
  wem_agent_install_file: Citrix Workspace Environment Management Agent Setup.exe
  wem_agent_install_arguments: "{{ wem_agent_versions[wem_agent_version].install_arguments }}"
  wem_agent_product_id: "{{ wem_agent_versions[wem_agent_version].product_id }}"
  wem_agent_install_state: present

=======

The wem_agent_versions hash table is used to look up a default set of installation arguments and the product id of the
version being installed.

It's generally not necessary to modify the variables defined in the hash table. There are other variables that can be used to
override the default value and use a desired value instead.

For example:

  wem_agent_install_arguments: /quiet Cloud=0

  can be used to override the value in the hash table:  (2012 in this example)

  wem_agent_versions.2012.install_arguments: /quiet Cloud=1

An SMB location must defined with the Ansible Become User having a minimum of Read-Only access to the Share and folder structure.
The smb_location variable can be defined in the group_vars, host_vars, or even with in the playbook itself.

Sample SMB File and Folder Structure:
  \\\\file1\\automation\\
      Citrix\\
          Workspace Environment Management\\
              2012\\
                  Citrix Workspace Environment Management Agent Setup.exe
              2103\\
                  Citrix Workspace Environment Management Agent Setup.exe

The wem_agent_version variable is used to control which version of the Citrix License will be installed.
This variable is used as the lookup key for the wem_agent_versions hash table.

Possible Values are:
  2009                      : This will install the 2009 version of the Citrix Workspace Management Agent
  2012                      : This will install the 2012 version of the Citrix Workspace Management Agent
  2103            (default) : This will install the 2103 version of the Citrix Workspace Management Agent

The wem_agent_smb_location variable aggregates the smb_location with the folder location of the WEM Console installation
file including the variable wem_agent_version as part of the folder location. The wem_agent_smb_location
variable can be customized in the group_vars, host_vars, or even with in the playbook itself.


wem_agent_install_file: Citrix Workspace Environment Management Agent Setup.exe
wem_agent_install_arguments: "{{ wem_agent_versions[wem_agent_version].install_arguments }}"
wem_agent_product_id: "{{ wem_agent_versions[wem_agent_version].product_id }}"
wem_agent_install_state: present

The wem_agent_install_file variable is the installatin file that will be used during the installation process.
The wem_agent_install_file variable can be customized in the group_vars, host_vars, or even with in the playbook itself.

The wem_agent_install_arguments variable contains the installation arguments that will be used for the installation.
By default, the wem_agent_install_arguments variable uses the value defined in the wem_agent_versions hash table using
wem_agent_version as the hash key. The wem_agent_install_arguments variable can be customized in the group_vars,
host_vars, or even with in the playbook itself.

The wem_agent_product_id variable contains the product id relative to the version of the License
that is being installed. By default, the .iso file used is defined in the wem_agent_versions hash table using
wem_agent_version as the hash key. The wem_agent_product_id variable can be customized
in the group_vars, host_vars, or even with in the playbook itself.

The wem_agent_install_state variable controls the installation state for the Citrix WEM Console.  This
role does not currently support the removal of the Citrix Workspace Management Agent or any of the
pre-requisites and plug-ins that were installed during the installation process.

Possible Values are:
  present         (default) : This will install the License Server

While the defaults/main.yml file can be modified directly, future updates will
overwrite the defaults/main.yml file.  In addition, many of the roles use the same
variables and could benefit from the variable being set at a high precedence.
The variable smb_location is one such example.

To override the default variable values the best place(s) to accomplish this is
to set the new values in one or more of the following locations:
  host_vars/{hostname.mycompany.com}.yml
  group_vars/{group}.yml
  group_vars/all.yml
  playbook.yml (Best option if you need to import multiple certificates)

Dependencies
------------

No dependencies.

Example Playbook
----------------

    - hosts: servers
      roles:
         - ctxcommunity.wem.install_agent

    - hosts: servers
      roles:
         -  role: ctxcommunity.wem.install_agent
            vars:
              wem_agent_version: 2012

License
-------

GPL-3.0-only

Author Information
------------------

Joe Shonk
