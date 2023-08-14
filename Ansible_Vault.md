Using Ansible vault We can Encrypt an Entire Playbook. This is usedful when we have sensitive data inside the playbook 

1. To Create an encrypted playbook 

    ansible-vault create pb_name.yaml

 When Creating a playbook. Ansible vault will ask for a password. This will be required during execution

 2. To Run ansible Playbook 
    
    ansible-playbook pb_name.yaml --ask-vault-pass 

 3. View Vaulted Yaml file. 

     ansible-vault view pb_name.yaml 

 4. Edit a Vaulted Yaml File. 

    ansible-vault edit pb_name.yaml

 5. Get list of options 

    ansible-vault --help 