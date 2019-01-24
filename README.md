Ansible Role: oracle
====================

This role combines a bunch of variables used by other Oracle related roles.

Supported OS:
-------------
* RedHat
* CentOS
* OracleLinux

Requirements
------------

None

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    # defaults file for oracle common role

    # oracle OS user and group
    oracle_user: oracle
    oracle_oinstall_group: oinstall
    oracle_dba_group: dba

    # oracle nagios user
    oracle_nagios_user: nagios

    # scripts directory
    oracle_scripts_dir: /home/{{ oracle_user }}/scripts
    oracle_sql_scripts_dir: "{{ oracle_scripts_dir }}/sql"
    oracle_log_scripts_dir: "{{ oracle_scripts_dir }}/log"

    # facts directory
    oracle_facts_dir: "{{ oracle_scripts_dir }}/facts.d"

    # Grid Infrastructure info
    oracle_gi_info: '{{ oracle_gatherinfo_gi_gi_info|default({}, true) }}'

	# 'oracle_gi_info' might be either populated dynamically (autodiscovery) using 'oracle-gatherinfo-gi' role, or set manually:
    # e.g.:
    #oracle_gi_info:
    #  oracle_home: "/u01/app/12.1.0/grid"
    #  rac_nodes: []
    #  rac_remote_nodes: []
    #  software_version: "12.1.0.2.0"
		
    # List describing the databases hosted on the servers
    oracle_databases: '{{ oracle_gatherinfo_databases_oracle_databases|default([], true) }}'

	# 'oracle_databases' might be either populated dynamically (autodiscovery) using 'oracle-gatherinfo-databases' role, or set manually:
    # e.g.:
    #oracle_databases:
    #  - cluster_database: "false"
    #    database_role: "PRIMARY"
    #    database_type: "SINGLE"
    #    db_name: "ORCL"
    #    db_unique_name: "ORCL"
    #    edition: "Enterprise"
    #    instance_name: "ORCL"
    #    instances: "ORCL"
    #    is_registered_in_gi: "true"
    #    oracle_home: "/u01/app/oracle/product/11.2.0.4/dbhome1"
    #    software_version: "11.2.0.4.0"
	#  - (...)
	
	
    # List describing Oracle listeners registered in GI OHAS
    oracle_registered_listeners: '{{ oracle_gatherinfo_listener_registered_listeners|default([], true) }}'

    # List describing active/running Oracle listeners
    oracle_running_listeners: '{{ oracle_gatherinfo_listener_running_listeners|default([], true) }}'

    # List describing db console services registered in GI OHAS
    oracle_dbconsole_registered_services: '{{ oracle_gatherinfo_dbconsole_registered_services|default([], true) }}'

    # List describing active/running db console services
    oracle_dbconsole_running_services: '{{ oracle_gatherinfo_dbconsole_running_services|default([], true) }}'

    # Default directory used for different non-database backups, such as ASM metadata, OCR registry backup, etc
    #   /u01/app/psu/backup         - for standalone installation
    #   /u01/app/oracle/psu/backup  - for RAC
    oracle_default_backup_dir: "{% if oracle_gi_info.rac_nodes|default('',true)|length == 0 %}/u01/app/psu/backup{% else %}/u01/app/oracle/psu/backup{% endif %}"

    # Default directory used for different log files
    #   /u01/app/psu/log            - for standalone installation
    #   /u01/app/oracle/psu/log     - for RAC
    oracle_default_log_dir: "{% if oracle_gi_info.rac_nodes|default('',true)|length == 0 %}/u01/app/psu/log{% else %}/u01/app/oracle/psu/log{% endif %}"

    # Default directory used for installation files
    oracle_default_stage_install_dir: /u01/app/oracle/install

    # RMAN catalog connection string
    oracle_rmancat_connection_string: "rmancat/secret@RMANCAT"



Dependencies
------------

None

Example Playbook
----------------

`N/A`

This role is not used explicitly. It is referred in `meta.yml` in other Oracle related roles.

Inside `vars/main.yml` or `group_vars/..` or `host_vars/..`:

    #----------------------------------
    # overrides role 'oracle' variables
    #----------------------------------

    # oracle OS user and group
    oracle_user: oracle
    oracle_oinstall_group: dba
    oracle_dba_group: dba

    # RMAN catalog connection string
    oracle_rmancat_connection_string: "rmancat/otherpass@RMANCAT"

    # ... etc ...


License
-------

GPLv3 - GNU General Public License v3.0

Author Information
------------------

This role was created in 2018 by [Krzysztof Lewandowski](mailto:Krzysztof.Lewandowski@fastmail.fm).

