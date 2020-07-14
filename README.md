# Network Automation using Ansible

### Ansible agentless architecture
Does not requires a custom agent on managed hosts

ANSIBLE_CONFIG enviroment variable points to ansible config file. 

SOFTWARE PACKAGE

MODULE CAN BE APPLIED TO TARGETED MODULES ONLY.

Modules which are not present and need to be run on hosts only, are not idempotent.

Command, shell, raw can be used to execute commands on hosts.

Command and Shell modules require a working Python installation on the managed host.

We can install apache servers using the module package as well on targeted group.
Example: ansible webservers -m package -a "name=httpd state=present"

To remove something, change the state to "absent"

## Ansible consists of following concepts

### Tasks:
Granular component of the ansible, part of the playbook focusing on statement and hosts.

### Playbook
Playbook can have multiple modules.

Playbook are YAML files with an extension .yml.

The command ansible-playbook playbookname.yml runs a playbook file.

--limit can use to target specific groups within a YAML file.
Example: ansible-playbook playbook_name.yml  --limit group_name

A syntax check can be run on the playbook which will verify that it can be ingested by ansible.
ansible-playbook syntax-check playbook_name.yml

Playbook can be validated by performing a dry run which will display what changes would occur if the playbook is executed.
ansible-playbook -C playbook_name.yml

### Inventory
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

Defining parent group.

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



- Defining Ranges

192.168.[4:7].[0:255] - Covers the range 192.168.4.0 to 192.168.7.255
[a.c].dns.com - Covers a.dsn.com till c.dns.com


It is possiible to convert INI into YAML files

- Verifying inventory

"ansible a.dns.com --list-host"

### Variables
Are used to
1> To create, modify or delete users
2> To install or uninstall software
3> To start, stop or restart services
4> To extract or retrieve from the internet
5> To create, modify or remove files
6> Install / Uninstall Packages

Scope of variables
Global: For all hosts
Host: For particular hosts only
Play: For context of current play only

### Vault
Vault is used to store
  - Passwords
  - API keys
  - Other secrets
  

Create
Anisble-vault create filename
 
View
Ansible-vault view filename

and Edit Encrypted file
Ansible-vault edit filename 

Encrypt / Decrypt already existing file
Ansible-vault encrypt / decrypt filename

Provide the vault with an password
Ansible-playbook --vault-id @prompt filename

Multiple vault passwords
Ansible-playbook --vault-id vars@prompt --vault-id playbook@prompt site.yml

Change the password of an encrypted file.
Ansible-playbook rekey filename

Suppressing sensitive output
Add "no_log:true"

### Handlers
Handlers are inactive tasks that respond to a notification explicitly invoked.
Tasks only notify their handlers when the task changes something on a managed host.
Each handler has a globally unique name and is triggered at the end of a block tasks in a playbook.
If not, task notifies the handler by name, then the handler will not run.
Handler runs once only after the task/s is/are completed.
Handlers are tasks, so same module can be utilised that is being used for task.
Normally, handlers are used to reboot hosts and restart services.

### Error Handlers
Blocks are clauses that logically group tasks as a unit.
They can be used to control how tasks are executed.
Blocks can also be used to back out of a change if tasks in the block fail.

A block can have a set of tasks grouped in rescue statement.
The tasks in the rescue statement only run if a task in the block fails.
Normally, the tasks in the rescue statement will recover the managed host from the failure.
This is useful if the block exists because multiple  tasks are needed to accomplish something.

Block also supports a third section, always.
After the block runs, and the rescue if there was a failure in block the tasks  in always run.
These tasks always run if the block runs at all, but they are subject to the conditional on the block.
To summarize:
Block: Defines the main tasks to run
Rescue: Defines the tasks to run if the tasks defined in the block clause fail.
Always: Defines the tasks that will always run independently of the success or failure of tasks defined in the block and rescue clauses.

### Jinja2 Template
Jinja2 template allows you to deploy a template that contains variable like an ansible playbook. 
Those variables are replaced with their values when the template is deployed. 

Template, Filter, Lookup plugin

### Ansible ROles
Creating Roles
Ansible roles allow you to make automation code more reusable .
Provide packaged tasks that can be configured through variables.
The playbook just calls the role and passes it the right values through its variables. 
Allow you to create generic code for one project and reuse it for other projects.

Advantages
Roles group content, allowing easy sharing of code with others
Roles can be written in a way that define the essential elements of a system type: web server
Roles make larger projects more manageable
Different administrator can develop roles in parallel


Creating a role
Create the role directory structure
Define the role content
Use the role in play.

Never share secret in a role as these are meant to be shared.

### Miscellaneous commands
Copy: To copy a file to managed hosts
File: To make sure that the file, directory, link exists 
Synchronize: To copy entire content of a directory
Lineinfile: To make sure certain line exists in a file. 
