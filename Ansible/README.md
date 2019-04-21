# Steps for using and Installing Ansible

### 1. Run This command for installing Ansible via present repo

    `sudo apt install ansible`

### 2. As Ansible use python libraries , use to install python

    `sudo apt install python`

### 3. Ansible uses SSH for connecting host to remote nodes, Install SSH via :

    `sudo apt install ssh`

### 4. For each remote node or vm we need to have usrname and password for connecting via SSH, It requires SSHpass to be installed

    `sudo apt install sshpass`

## Configuring Hosts File

There is a global hosts file at location > "/etc/ansible/hosts". This file contains all hosts information , DataBase Servers and more.
This file contains groups. Groups are like same type of servers. It can be like cassandra servers, hosting servers, or a companies server. Like

```
[virtualMachine]
192.168.99.101
192.168.99.100

[simba]
192.168.99.112
192.168.99.134

```

Here the group Name is surrounded by close bracket [] , we can simply pass virtualMachine for running on VM and simba for running on Simba .

# AD-Hoc Commands

 We can use commands to run on remote machine using ansible in local host terminal. This way we can execute simple and single tasks.

  ```
   ansible host/hostsGroup/hosts -m shell -a 'command to run'

   ansible dockerMachine3 -m ping 

   ansible dockerMachine3 -m shell -a "ps -a"

   ansible dockerMachine3 -m shell -a "docker ps" 
  ```
  

# PlayBooks

 Ansible provides a way to write scripts using yml language. We can write playbooks for automating a task . Task can be like checking status of server, updating the packages, installing new packages, removing and a lot more. This way we can save time by running a single playbook on multiple remote servers ranging from 2 to 1000, 10000 and more we require as per requirement.

 ## Sample Playbook

 ```
    - hosts: virtualMachine

      tasks:
      - shell : echo "Your first playbook works !"
        register : echo

      - debug: var=echo.stdout_lines
``` 
This playbook will echo *Your First Playbook* on Host terminal for each virtual server in virtualMachine group

## Difference between Shell and Command module
---

Shell module runs command in user shell directly with native commands whereas Command module runs command without user shell
Using command we do not need to remember active directory for user, we can execute command directly in any directory.

## For running a command in a specific folder/directory in remote vm
 ---
 ` command: chdir=/home/docker docker-compose up`

 ### This command will first change to */home/docker* directory and run command *docker-compose up*

## Configuring SSH for remote servers
---
We just need to add username and password against each server we need to connect with ansible in hosts file, like

```
[dockerMachine]
192.168.99.101 ansible_ssh_pass='tcuser' ansible_ssh_user='docker'
192.168.99.100 ansible_ssh_pass='test' ansible_ssh_user='test'
```
here *ansible_ssh_user* is for username and *ansible_ssh_pass* is for password.

# Docker Machine

### For creating Virtual Machines we use Docker-machine. Docker machine helps in creating machines with different configurations, like:

- Amazon Web Services
- Microsoft Azure
- Digital Ocean
- Exoscale
- Google Compute Engine
- Generic
- Microsoft Hyper-V
- OpenStack
- Rackspace
- IBM Softlayer
- Oracle VirtualBox
- VMware vCloud Air
- VMware Fusion
- VMware vSphere
- VMware Workstation (unofficial plugin, not supported by Docker)
- Grid 5000

## Python Support for VM
---
As Ansible depends on python for running modules , you need to have python on VM
You can install python on docker-machine vm by command

   ` tce-load -wi python `

## Giving names and Printing messages for tasks
---
### 1. We can name a task easily by using name property

  ```
  - name: Check Running Processes
    command: ps -a   
  ```

### 2. For direct printing a message we can write

 ```
 - debug:
     msg: 'process completed'
 ```

### 3. For debugging purpose we can register a command using a variable name and bind the output of that command to the variable

 ```
  - name: Check Running Processes
    command: ps -a   
    register: process

  - debug: var=process.stdout_lines
 ```

 > **Note** Use stdout for single line and stdout_lines for new line printing

### 4. For passing variables we can use interpolation 

 ` - debug: var= {{ process.stdout_lines }}` 

### 5. We can declare custom variables and reuse in playbook like

 ```
 vars:
  http_port:80
  remote_ip: 120.56.78.90
  remote_user: user

 command: ping {{ remote_ip }}    
 ```    

## Using Block and Rescue to alert/handle failed tasks
---
We use *Block* & *Rescue* in our playbooks to divide tasks into blocks. Blocks are part of playbook that does a specific job or list of jobs. We can use rescue option along with block to know if a task failed and send notification to slack/mail or any we have configured.
below is how you can handle with Blocks and if failed send alert to slack
```
tasks:
  - block:
      - here
      - go
      - all
      - your
      - tasks
    rescue:
      - name: notify slack of deploy failure
        local_action:
          module: slack
          token: "{{slack_token}}"
          msg: "Deploy failed on {{inventory_hostname}}"
```
 ## Sending Notifications to Channels like Mail, Slack , SMS
---
 ### 1. For sending Email using Gmail SMTP or local mail server

  ```
    - name: Sending an e-mail using Gmail SMTP servers
        mail:
            host: smtp.gmail.com
            port: 587
            username: user@mail.com
            password: user password
            to: User <user@mail.com>
            subject: Subject which tells the process or playbook run 
            body: Messsage body
    - name: Send e-mail to a bunch of users, attaching files
        mail:
            host: 127.0.0.1
            port: 2025
            subject: Ansible-report
            body: Hello, this is an e-mail. I hope you like it ;-)
            from: jane@example.net (Jane Jolie)
            to:
            - John Doe <j.d@example.org>
            - Suzie Something <sue@example.com>
            cc: Charlie Root <root@localhost>
            attach:
            - /etc/group
            - /tmp/avatar2.png
            headers:
            - Reply-To=john@example.com
            - X-Special="Something or other"
            charset: us-ascii
        delegate_to: localhost

    - name: Sending an e-mail using the remote machine, not the Ansible controller node
        mail:
            host: localhost
            port: 25
            to: John Smith <john.smith@example.com>
            subject: Ansible-report
            body: System {{ ansible_hostname }} has been successfully provisioned.

    - name: Sending an e-mail using Legacy SSL to the remote machine
        mail:
            host: localhost
            port: 25
            to: John Smith <john.smith@example.com>
            subject: Ansible-report
            body: System {{ ansible_hostname }} has been successfully provisioned.
            secure: always

    - name: Sending an e-mail using StartTLS to the remote machine
        mail:
            host: localhost
            port: 25
            to: John Smith <john.smith@example.com>
            subject: Ansible-report
            body: System {{ ansible_hostname }} has been successfully provisioned.
            secure: starttls        
  ```

### 2. For Sending to Slack Channel

 ```
 - name: Send notification message via Slack
    slack:
        token: thetoken/generatedby/slack
        msg: '{{ inventory_hostname }} completed'
    delegate_to: localhost

 - name: Send notification message via Slack all options
    slack:
        token: thetoken/generatedby/slack
        msg: '{{ inventory_hostname }} completed'
        channel: #ansible
        username: 'Ansible on {{ inventory_hostname }}'
        icon_url: http://www.example.com/some-image-file.png
        link_names: 0
        parse: 'none'
    delegate_to: localhost

 - name: insert a color bar in front of the message for visibility purposes and use the default webhook icon and name configured in Slack
    slack:
        token: thetoken/generatedby/slack
        msg: '{{ inventory_hostname }} is alive!'
        color: good
        username: ''
        icon_url: ''

 - name: Use the attachments API
    slack:
        token: thetoken/generatedby/slack
        attachments:
            - text: Display my system load on host A and B
              color: #ff00dd
              title: System load
              fields:
                - title: System A
                  value: "load average: 0,74, 0,66, 0,63"
                  short: True
                - title: System B
                  value: 'load average: 5,16, 4,64, 2,43'
                  short: True

 - name: Send a message with a link using Slack markup
    slack:
        token: thetoken/generatedby/slack
        msg: We sent this message using <https://www.ansible.com|Ansible>!

 - name: Send a message with angle brackets and ampersands
    slack:
        token: thetoken/generatedby/slack
        msg: This message has &lt;brackets&gt; &amp; ampersands in plain text.
 ```

### 3. For sending a voice notification, needs say or espeak 
  We can use speech synthesizer to generate audio from text for genrating voice events.

 ```
  - say:
    msg: '{{ inventory_hostname }} is all done'
    voice: Zarvox
  delegate_to: localhost
 ```