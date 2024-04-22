Important Things to rember when writing Ansible Playbooks. 

__Ansible Configuration File__ 

We can create ansible file in project directory. Key things specify here are inventroy, roles and collections paths , privilege escalation path. See sample ansible.cfg file.
In Roles path we have  our own roles created. When path is added in roles path we can use those roles on that path in our playbook. Roles  download from external such as ansible galaxy specified in the collections path.


When creating inventory We can group host under separate group using children for example. Prod grp is memeber of webservers. In that case we can use as below.

[prod]
4.194.28.139

[webservers:children]
prod
See inventory file example in the repo. 

__Role Usage__ 

1. To Create go to Role directory
2. Issue ansible-galaxy init role_name which will create the role strcuture
      ex: ansible-galaxy init apache 
3. In the role skelton Generated following folders 

  __defaults__: This folder typically contains default variables for the role. These variables are used unless overridden by variables provided by the user.
  __files__: This folder is used for storing static files that need to be transferred to the target hosts.
  __handlers__: Handlers are tasks triggered by other tasks. This folder contains handler definitions. (Ex: Restart Apache) 
  __meta__: The meta folder can contain role metadata, such as dependencies.
  __tasks__: This folder contains the main tasks of the role, defining what actions the role should perform.
  __templates__: If the role involves generating configuration files dynamically, this folder contains Jinja2 templates for those files. We can specify Templates that should be deployed
  __tests__: This folder can include tests for the role, such as integration tests or automated tests.
  __vars__: This folder can contain additional variables used by the role, similar to the defaults folder, but with lower precedence.

Once the role is Written respective folders above We can use the role as below in the playbook.

---
- name: Configure apache
  hosts: webservers

  roles:
     - apache

__Ansible Facts USage__ 

Ansible facts used to get infomation about the target host. To get facts We can use below adhoc commands 
       ansible host_name_in_inventory  -m gather_facts 
       ansible host_name_in_inventory  -m setup


Then we can use the facts in templates and playbooks. Refer facts usage playbook 1 and 2  in the  repo. Using facts we can defined conditions. (When ansible_os_family == "RedHat"  perform the task ) 

In the facts usage 1 playbook 

IT will deploy template only to inventory host dev using the inventory variable inventory_hostname. 

In the host.j2 file it is go through all the hosts and specify the facts in the template 


In the facts usage 2 playbook it will replace the content using ansible facts. 


If Under the facts if multiples are defined like below and want to use address, 

"ansible_default_ipv4": {
    "address": "172.18.79.163",
    "alias": "eth0",
    "broadcast": "172.18.79.255",
    "gateway": "172.18.64.1",
    "interface": "eth0",
    "macaddress": "00:15:5d:58:3e:62",
    "mtu": 1432,
    "netmask": "255.255.240.0",
    "network": "172.18.64.0",
    "prefix": "20",
    "type": "ether"
}


We have specify like like this in the playbook as variable

{{  ansible_default_ipv4.address  }}

__Use of Ansible Vault and Variable in seperate file to to Create Users with the Password__ 

1. To Create a Ansible vault the provide password
    ansible-vault create vault.yml

2. The Create user list as dictionary. In the case userlist file contain Users as heading  Using We can through the variables.

3. Using the inventory_hostname inventory variable specify condition to Deploy specify host

4. Refer playbook usercreationwithvault.yml for more understadning in the repository 


__Sample Apache Role__

Refer "apache" folder for sample apache installation role

__Evaulate Conditions__ 

Using when we can deifned conditions. 

Using AND we can combine conditions 

When checking for numeric vaules using single quotes
       when: ansible_lvm.vgs.rootvg.free_g > '30' 

Using defined and undefine we can check whether facts is defined or not and then do something
For Example refer factsusage2.yml playbook 

For Better understanding of when usage refer lvms_with_conditions.yml playbook 


__Ignore Errors usage __ 

By default playbook stops when task failed. To avoid this we can use ignore_errors: yes In this case task file skip even if it fails

__To Run Playbook__ 

When using ansible-core we have to use below command 

ansible-playbook pb.yml

When using AAP use below

ansible-navigator rum -m stdout pb.yml 

By default ansible.cfg in the project directory will be taken as the  configuration in some case (In Ubuntu install machine)  it wil not take as it is. In that case use below command to specify the ansible.cfg file 

      export ANSIBLE_CONFIG=ansible.cfg



