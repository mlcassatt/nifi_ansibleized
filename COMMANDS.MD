# Commands
## Ansible

Search for modules in the community to solve your problem

`ansible-galaxy search <keyword>`

Create a new role skeleton dir structure in proper form

`ansible-galaxy init <role-name>`

Look for documentation related to what you need to do while offline

`ansible-doc -l | grep <module task>`

Create an ansible vault for sensitive data

`ansible-vault create <file.yml>`

Run a playbook with a vault

`ansible-playbook --ask-vault-pass -i <inventory> <playbook>`


## RHEL-ish
Generate ssh keys for a user

`ssh-keygen`

Edit sudoers file to enable user to su-do 

`visudo`

Change fqdn of host

`hostnamectl set-hostname <new-fqdn>`


## Simplified Git Process
Get a copy of a repo of interest from the team

`git clone <repo>`

Make a branch from your new copy and switch to that branch (local) named “newbranch”

`git checkout -b newbranch`

Check the status of changes on current (local) branch

`git status`

Add specific files to the branch for commit

`git add <files>`

Look at differences between current files and branch committed files

`git diff <file>`

Commit changes that have been _added_ to the (local) branch

`git commit -m “Removed play for testing”`

Push changes to remote repository under your new branch named “newbranch”

`git push -u origin newbranch`

## NiFi
Main properties file for most configuration of NiFi

~nifi/conf/nifi.properties

Main location to change authorization and authentication information for nifi (must remove users.xml and authorizations.xml and restart NiFi to take affect!)

~nifi/conf/authorizer.xml

User identities who can interact with NiFi (Must be deleted if changes are made to  authorizer.xml)

~nifi/conf/users.xml

Policies for what a user identified in users.xml can do in NiFi UI

~nifi/conf/authorizations.xml


## Ansible
Local ansible configuration/modules storage dir

~/.ansible

Ansible configuration file - local directory version used if exists

ansible.cfg

An inventory file (any name) to apply changes to specified systems

inventory*

Host-named variable files to automatically pick up for host specific needs

host_vars/

Group-named variables files to automatically pick up for group name specific needs

group_vars/


## Helpful Links
Ansible Docs 
* https://docs.ansible.com/ansible/latest/index.html
  * Ansible Best Practices
  * Variable precedence
  * Modules Index
  * Roles
  * ansible-vault
  * Network management
  * Dynamic inventory

pfSense automation

https://github.com/opoplawski/ansible-pfsense

Helpful NiFi configuration for multi-tenancy

https://bryanbende.com/development/2016/08/17/apache-nifi-1-0-0-authorization-and-multi-tenancy
