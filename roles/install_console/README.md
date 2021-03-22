Role Name
=========

Role Name: ctxcommunity.wem.install_console

This role is used to Install the Citrix Workspace Management Console from an SMB share.

Requirements
------------

The Microsoft .NET 4.8 Framework is required for most versions.

Role Variables
--------------

The defaults/main.yml contains the following variables and default values:

  wem_console_versions:
    2006:
      install_arguments: '/s /v"/qn"'
      product_id: '{975909FA-7489-4BF6-BFF0-28EBE4FF0C34}'
    2009:
      install_arguments: '/s /v"/qn"'
      product_id: '{753C457B-BA47-4CC5-9B1A-FD382D3C5571}'
    2012:
      install_arguments: '/s /v"/qn"'
      product_id: '{FE8FE4D5-89A2-4C30-BF2A-975D8EA7CEA0}'
    2103:
      install_arguments: '/s /v"/qn"'
      product_id: '{BC8BD1DA-12E1-46B6-AA16-27E8DD2B9CB7}'

  smb_location: \\\\file1\\automation

  wem_console_version: 2103
  wem_console_smb_location: "{{ smb_location }}\\\\Citrix\\\\Workspace Environment Management\\\\{{ wem_console_version }}"
  wem_console_install_file: Citrix Workspace Environment Management Console Setup.exe
  wem_console_install_arguments: "{{ wem_console_versions[wem_console_version].install_arguments }}"
  wem_console_product_id: "{{ wem_console_versions[wem_console_version].product_id }}"
  wem_console_install_state: present

=======

The wem_console_versions hash table is used to look up a default set of installation arguments and the product id of the
version being installed.

It's generally not necessary to modify the variables defined in the hash table. There are other variables that can be used to
override the default value and use a desired value instead.

For example:

  wem_console_install_arguments: '/s /v"/qn"'

  can be used to override the value in the hash table:  (2012 in this example)

  wem_console_versions.2012.install_arguments: '/s /v"/qn"' /LOG c:\\temp\\install.log

An SMB location must defined with the Ansible Become User having a minimum of Read-Only access to the Share and folder structure.
The smb_location variable can be defined in the group_vars, host_vars, or even with in the playbook itself.

Sample SMB File and Folder Structure:
  \\\\file1\\automation\\
      Citrix\\
          Workspace Environment Management\\
              2012\\
                  Citrix Workspace Environment Management Console Setup.exe
              2103\\
                  Citrix Workspace Environment Management Console Setup.exe

The wem_console_version variable is used to control which version of the Citrix License will be installed.
This variable is used as the lookup key for the wem_console_versions hash table.

Possible Values are:
  2009                      : This will install the 2009 version of the Citrix Workspace Management Console
  2012                      : This will install the 2012 version of the Citrix Workspace Management Console
  2103            (default) : This will install the 2103 version of the Citrix Workspace Management Console

The wem_console_smb_location variable aggregates the smb_location with the folder location of the WEM Console installation
file including the variable wem_console_version as part of the folder location. The wem_console_smb_location
variable can be customized in the group_vars, host_vars, or even with in the playbook itself.

wem_console_install_file: Citrix Workspace Environment Management Console Setup.exe
wem_console_install_arguments: "{{ wem_console_versions[wem_console_version].install_arguments }}"
wem_console_product_id: "{{ wem_console_versions[wem_console_version].product_id }}"
wem_console_install_state: present

The wem_console_install_file variable is the installatin file that will be used during the installation process.
The wem_console_install_file variable can be customized in the group_vars, host_vars, or even with in the playbook itself.

The wem_console_install_arguments variable contains the installation arguments that will be used for the installation.
By default, the wem_console_install_arguments variable uses the value defined in the wem_console_versions hash table using
wem_console_version as the hash key. The wem_console_install_arguments variable can be customized in the group_vars,
host_vars, or even with in the playbook itself.

The wem_console_product_id variable contains the product id relative to the version of the License
that is being installed. By default, the .iso file used is defined in the wem_console_versions hash table using
wem_console_version as the hash key. The wem_console_product_id variable can be customized
in the group_vars, host_vars, or even with in the playbook itself.

The wem_console_install_state variable controls the installation state for the Citrix WEM Console.  This
role does not currently support the removal of the Citrix Workspace Management Console or any of the
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
         - ctxcommunity.wem.install_console

    - hosts: servers
      roles:
         -  role: ctxcommunity.wem.install_console
            vars:
              wem_console_version: 2012

License
-------

GPL-3.0-only

Author Information
------------------

Joe Shonk
