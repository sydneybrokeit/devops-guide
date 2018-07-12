# Core concepts
## Playbooks
Playbooks contain the full set of things to be done to a set of hosts.

## Hosts
Hosts are the machines to be configured using Ansible.

## Inventory
The inventory contains the list of hosts that will be configured.

To specify a host in an ini-style inventory file:
````
hostname
````

You can also specify host-level variables, such as as username, password, ip address, etc, by specifying them after the hostname:
````
hostname ansible_host=x.y.z.a
````


### File formats
Ansible supports ini-style and yml inventory files.

### Groups
Groups can be defined in the inventory file.

````
[group]
host1
host2
````

Groups can also be comprised of other groups.
````
[group1]
hostA
hostB

[group2]
hostC
hostD

[group3]
group1
group2
````


### Using an inventory
````ansible-playbook -i inventory.file playbook.yml````


# Roles
- Discreet series of steps are best left as part of a **role**.  By default, a playbook will search in ````./roles```` for any specified roles, then will search in the local ansible directory (where ansible-galaxy roles are stored).

## Using a role
To use a role within a playbook, you can set it one of two ways.

````
roles:
  - a
  - b
````

````
roles:
  - { role: a }
  - { role: b }
````

I personally use the second format, because it allows for adding further parameters later on as needed (such as ````become```` or adding a tag).

- Make roles reusable.  If you need a specific version of, i.e., nodejs, write logic that will make it easier to use a different version later (for example, use version variables instead of hard-coding the download URL for node.)

````
vars:
  node_version: 4.0.0
  node_file: "nodejs-v{{ node_version }}-{{ host_os }}.tar.gz"
  node_download_url: "http://whatever.node.com/downloads/{{ node_file }}"
````

## Role directory structure
A role can contain any or all of the follow 6 directories:
- **defaults** : default values for variables if none is provided
- **files** : files used within the role
- **handlers** : handlers used within the role
- **meta** : descriptions about the role (such as dependencies)
- **tasks** : the actual tasks to perform
- **templates** : j2 templates used by the role
- **vars** : variables for the role

Inside ````defaults````, ````handlers````, ````meta````, ````templates````, and ````vars````, you can include a ````main.yml```` file that will be loaded automatically as part of the role. 

## Ansible Galaxy
- Use ansible-galaxy roles where possible.  Save the time and energy for productive work.

````
ansible-galaxy install geerlingguy.apache
````

- To update an ansible-galaxy role:

````
ansible-galaxy install --force geerlingguy.apache
````

