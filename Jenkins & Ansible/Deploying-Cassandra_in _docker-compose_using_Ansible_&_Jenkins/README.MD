# Deploying Cassandra with Jenkins And Ansible

## Prerequisites

 1. Ansible plugin for Jenkins
 2. Docker-py pip package for Ansible
 3. A VM configured to use with Ansible (if you running on same machine, you can skip this step.)

## Ansible modules used

### 1. *docker_service* module - Manage docker services and containers.
 - Uses Docker-compose to start, shutdown and scale services.
 - Works with compose version 1 and 2
 - You can provide a docker-compose.yml (or .yaml) file or write inline using *definition* option as we did in our example.

## Steps for configuring Jenkins Task, Creating Ansible Playbook with Docker-Service module
---
### Step 1. Ansible Playbook

- First create a playbook with all requirements you need. In this folder we have *cassandra-deploy-run.yml* & *cassandra-deploy-stop.yml* files for running and stopping docker-compose services.

- Use *cassandra-deploy-run.yml* for running **Cassandra** as a Docker-Compose service.
- Use *cassandra-deploy-stop.yml* to stop **Cassandra** Docker-Compose service.

### Step 2. Jenkins Configuration

1. Create a new Jenkins Job and go to configure option.
2. Go to General Tab and Tick *This project is parameterized*.
3. Then Add Parameter and select *String Parameter*.
4. Enter name *port1* (for this example only , you may choose as required) and a default value like "7000" for port1 variable. You may enter description or leave empty. Repeat step and add port2, port3, port4, port5 similarly.
5. Then Add Build Step option, choose *Invoke Ansible Playbook* for connecting to playbook, we created previously.
6. Enter path/Location of yml file (playbook file) in Playbook path.
7. Select Inventory option if you having an inventory file , else choose *Do not specify Inventory*.
8. Click on Advanced > Additional Parameters field and add like:
 `--extra-vars "ansible_become_pass=Password port1='$port1' port2='$port2' port3='$port3' port4='$port4' port5='$port5'"`
9. Now click on Save to save and go back.
10. Now click on *Build with Parameters* option in sidebar. 
11. Enter parameters we created like 7000,7001 for ports or any of you have defined.
12. Click on Build to Execute and Docker will be deployed.
