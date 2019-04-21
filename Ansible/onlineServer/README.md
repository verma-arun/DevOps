# This file will guide you to setup Ansible on remote Machines.

## Hosts File

1. Here is a custom host file in *inventory* directory named as **hosts** that contains all servers ip addresses

Supply this host by passing arguments while running playbook like
 ` ansible-playbook name.yml -i inventory/hosts ` 
-i parameter is for supplying local inventory file

2. When Pyhthon not installed on machine we can use raw modules but that is not considered a good practice. So we use raw only for installing python or in cases when we do not have python on remote machine.

3. docker-install.yml will be for installing docker on all servers

4. docker-compose-install.yml will be for installing docker on all servers