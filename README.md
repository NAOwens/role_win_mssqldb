role_win_mssqldb 
=========

This role uses a template to build a file with the information needed to create a DB in MS SQL Server on Windows. The template file is placed on the server as DBCreate.sql and that file is used in the sqlcmd to install the DB.

Requirements
------------

Template file - DBCreate.sql.j2

Role Variables
--------------

Dependencies
------------


Example Playbook
----------------
- name: Create SQL directories
  ansible.windows.win_file:
    path: "{{ item }}"
    state: directory
  loop: '{{ db_dirs }}'
  tags: 
    - mssql_dbcreate

- name: Create sql script file to create {{ var_db_name }} DB
  ansible.windows.win_template:
    src: 'DBCreate.sql.j2'
    dest: '{{ inst_dbcreate_file }}'
  tags: 
    - mssql_dbcreate

- name: Create {{ var_db_name }} DB
  ansible.windows.win_command: "sqlcmd -U sa -P {{ db_sapwd }} -i {{ inst_dbcreate_file }}"
  tags: 
    - mssql_dbcreate

License
-------

GPL

Author Information
------------------

Norman Owens
Norman.Owens@redhat.com