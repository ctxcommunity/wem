Role Name
=========

Role Name: ctxcommunity.wem.install_broker

This role is used to Install the Citrix Workspace Management Infrastructure Services from an SMB share.

Requirements
------------

The Microsoft .NET 4.8 Framework is required for most versions.

Role Variables
--------------

The defaults/main.yml contains the following variables and default values:

  wem_broker_versions:
    2006:
      install_arguments: '/s /v"/qn"'
      product_id: '{59333D97-8DBE-4296-A5AE-D0233EA3C3D6}'
    2009:
      install_arguments: '/s /v"/qn"'
      product_id: '{42D7566D-7A86-409F-A392-85B5D24F7A24}'
    2012:
      install_arguments: '/s /v"/qn"'
      product_id: '{20DAF27F-18FE-457D-904D-EFE4A201FC62}'
    2103:
      install_arguments: '/s /v"/qn"'
      product_id: '{F9BFC2F6-2C58-4D10-8694-88CE4018B00E}'

  smb_location: \\file1\automation

  wem_broker_version: 2103
  wem_broker_smb_location: "{{ smb_location }}\\Citrix\\Workspace Environment Management\\{{ wem_broker_version }}"
  wem_broker_install_file: Citrix Workspace Environment Management Infrastructure Services Services Setup.exe
  wem_broker_install_arguments: "{{ wem_broker_versions[wem_broker_version].install_arguments }}"
  wem_broker_product_id: "{{ wem_broker_versions[wem_broker_version].product_id }}"
  wem_broker_install_state: present

=======

The wem_broker_versions hash table is used to look up a default set of installation arguments and the product id of the
version being installed.

It's generally not necessary to modify the variables defined in the hash table. There are other variables that can be used to
override the default value and use a desired value instead.

For example:

  wem_broker_install_arguments: '/s /v"/qn"'

  can be used to override the value in the hash table:  (2012 in this example)

  wem_broker_versions.2012.install_arguments: '/s /v"/qn"' /LOG c:\\temp\\install.log

An SMB location must defined with the Ansible Become User having a minimum of Read-Only access to the Share and folder structure.
The smb_location variable can be defined in the group_vars, host_vars, or even with in the playbook itself.

Sample SMB File and Folder Structure:
  \\\\file1\\automation\\
      Citrix\\
          Workspace Environment Management\\
              2012\\
                  Citrix Workspace Environment Management Infrastructure Services Setup.exe
              2103\\
                  Citrix Workspace Environment Management Infrastructure Services Setup.exe

The wem_broker_version variable is used to control which version of the Citrix License will be installed.
This variable is used as the lookup key for the wem_broker_versions hash table.

Possible Values are:
  2009                      : This will install the 2009 version of the Citrix Workspace Management Infrastructure Services
  2012                      : This will install the 2012 version of the Citrix Workspace Management Infrastructure Services
  2103            (default) : This will install the 2103 version of the Citrix Workspace Management Infrastructure Services

The wem_broker_smb_location variable aggregates the smb_location with the folder location of the WEM Console installation
file including the variable wem_broker_version as part of the folder location. The wem_broker_smb_location
variable can be customized in the group_vars, host_vars, or even with in the playbook itself.


wem_broker_install_file: Citrix Workspace Environment Management Infrastructure Services Setup.exe
wem_broker_install_arguments: "{{ wem_broker_versions[wem_broker_version].install_arguments }}"
wem_broker_product_id: "{{ wem_broker_versions[wem_broker_version].product_id }}"
wem_broker_install_state: present

The wem_broker_install_file variable is the installatin file that will be used during the installation process.
The wem_broker_install_file variable can be customized in the group_vars, host_vars, or even with in the playbook itself.

The wem_broker_install_arguments variable contains the installation arguments that will be used for the installation.
By default, the wem_broker_install_arguments variable uses the value defined in the wem_broker_versions hash table using
wem_broker_version as the hash key. The wem_broker_install_arguments variable can be customized in the group_vars,
host_vars, or even with in the playbook itself.

The wem_broker_product_id variable contains the product id relative to the version of the License
that is being installed. By default, the .iso file used is defined in the wem_broker_versions hash table using
wem_broker_version as the hash key. The wem_broker_product_id variable can be customized
in the group_vars, host_vars, or even with in the playbook itself.

The wem_broker_install_state variable controls the installation state for the Citrix WEM Console.  This
role does not currently support the removal of the Citrix Workspace Management Infrastructure Services or any of the
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
         - ctxcommunity.wem.install_broker

    - hosts: servers
      roles:
         -  role: ctxcommunity.wem.install_broker
            vars:
              wem_broker_version: 2012

License
-------

GPL-3.0-only

Author Information
------------------

Joe Shonk
