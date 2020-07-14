#Network Automation using Ansible

##Ansible consists of following concepts

###Tasks:
Granular component of the ansible which in turn is a part of a playbook.

###Playbook
Playbook can have multiple modules.

Playbook are YAML files with an extension .yml.

The command ansible-playbook playbookname.yml runs a playbook file.

--limit can use to target specific groups within a YAML file.
Example: ansible-playbook playbook_name.yml  --limit group_name

A syntax check can be run on the playbook which will verify that it can be ingested by ansible.
ansible-playbook syntax-check playbook_name.yml

Playbook can be validated by performing a dry run which will display what changes would occur if the playbook is executed.
ansible-playbook -C playbook_name.yml

###Inventory
Inventory is a list of hosts (Routers, Switches).

Hosts can be grouped together.

Groups can have subgroups.

Single host can be part of multiple groups.

Variables can be assigned to hosts and groups.

Static inventory can be written in INI or YAML.

Dynamic inventory are those which are automatically generated and updated.

Defining a nested group

INI Format of the file

[usa]
Abc.com
Def.com

[India]
Ghi.com
Jkl.com

[Ireland]
Mno.com
Pqr.com

# Defining parent group.

[asia:children]
India

[europe:children]
Ireland

.. Etc.

YAML Format of the file

All:
      children:
		Asia:
			Children:
				      India:
				                 Hosts:
						        a.dns.com
						        b.dns.com
                    Europe:
			 Children:
			                  Ireland:
			                                 Hosts:
			                                             lakaas.com
			                                             pipasj.com



# Defining Ranges

192.168.[4:7].[0:255] - Covers the range 192.168.4.0 to 192.168.7.255
[a.c].dns.com - Covers a.dsn.com till c.dns.com


# Its possiible to convert INI into YAML files

# Verifying inventory

"ansible a.dns.com --list-host"


