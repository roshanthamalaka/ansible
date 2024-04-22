Important Things to rember when writing Ansible Playbooks. 

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


Then we can use the facts in templates and playbooks. Refer facts usage playbooks in the  repo. 









