# Steps for creating a `Jenkins` job to run `ansible playbook` which will run multinode cluster of cassandra on remote-host.
` (remote-host)in my case remote-host is another system and connected by same network`.

## prerequisites :
    * Jenkins
    * Ansible
    * Ssh
    * Sshpass
    * Pip
    * Docker-Compose

## first create an hosts groupe and add the host info going to run how cassandra node
got to : 
```bash
cd etc/ansible
```
open hosts file and create groupe as bellow :

```bash
[groupeName]
hostIP ansible_ssh_user='host-user' ansible_ssh_pass='host-pass'
```
## now create a ansible playbook for running multinode cluster of cassandra, i have already created one:

### Note: I have used `when conditional statement` for running different configuration of cassandra on the bases of there IP's.

```bash
- name: Run Cassandra in Docker-Compose with inline v2 compose 
  hosts: remoteHosts, localhost
  gather_facts: yes
  become: true
  become_method: enable
  vars:               # declare here variables you want with
    - port1: "7000"   # key value pair like name: "value" for default
    - port2: "7001"
    - port3: "7199"
    - port4: "9042"
    - port5: "9160"
  tasks:
    - docker_service:
        project_name: cassandra-compose  # Name you want to Assign to Docker Stack running as Compose
        state: present     # State is for making up and down the docker-compose container, present is for up & absent for down
        definition:
          version: '2'    # version of docker-compose file syntax
          services:
            cassandra:    # Cassandra service 
              image: cassandra:3.9   # Name of image to Pull
              volumes:
                - /var/db/UrvaraApm/cassandra/:/var/lib/cassandra/data
              ports:
                - "{{ port1 }}:7000"  # variable ports that be supplied as arguments while running playbook
                - "{{ port2 }}:7001"
                - "{{ port3 }}:7199"
                - "{{ port4 }}:9042"
                - "{{ port5 }}:9160"
              environment:
                - CASSANDRA_BROADCAST_ADDRESS=192.168.43.78
      become: true      # For enabling Priviledge mode
      become_method: sudo  # To run command with sudo priviledges 
      register: output
      when: ansible_default_ipv4.address == "192.168.43.78"

    - debug:
        var: output

    - docker_service:
        project_name: cassandra-compose-2  # Name you want to Assign to Docker Stack running as Compose
        state: present     # State is for making up and down the docker-compose container, present is for up & absent for down
        definition:
          version: '2'    # version of docker-compose file syntax
          services:
            cassandra:    # Cassandra service 
              image: cassandra:3.9   # Name of image to Pull
              volumes:
                - /var/db/UrvaraApm/cassandra/:/var/lib/cassandra/data
              ports:
                - "{{ port1 }}:7000"  # variable ports that be supplied as arguments while running playbook
                - "{{ port2 }}:7001"
                - "{{ port3 }}:7199"
                - "{{ port4 }}:9042"
                - "{{ port5 }}:9160"
              environment:
                - CASSANDRA_SEEDS=192.168.43.78
                - CASSANDRA_BROADCAST_ADDRESS=192.168.43.112
      register: output
      when: ansible_default_ipv4.address == "192.168.43.112"

    - debug:
        var: output      
        
```

## Now we are creating a `Jenkins` job which is going to run the `Ansible playbook` of running cassandra multinode cluster.


