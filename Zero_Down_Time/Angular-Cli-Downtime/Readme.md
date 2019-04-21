[![N|Solid](https://automationpanda.files.wordpress.com/2018/01/angular-card.png?w=250)](https://cli.angular.io)
## Zero Down Time with Angular Application
**Zero Down Time using Blue-green deployment is a technique that reduces downtime and risk by running two identical production environments called Blue and Green.**
At any time, only one of the environments is live, with the live environment serving all production traffic. For this example, Blue is currently live and Green is idle.
As you prepare a new version of your software, deployment and the final stage of testing takes place in the environment that is not live: in this example, Green. Once you have deployed and fully tested the software in Green, you switch the router so all incoming requests now go to Green instead of Blue. Green is now live, and Blue is idle.

## System Requirements
  - Node Install
  - Angular Cli Installed
  - Docker Version  above 17.4

## Install Angular Cli
    $ npm install -g @angular/cli

## Steps to Achieve Zero Down Time

### Step 1 - Angular Production Build
Change Directory Root to Angular Application and try running the application to ensure it compiles correctly and displays in the browser:.

    $ cd AngularCli-Version1
    $ ng serve --open

If successfully displays in the browser, then build the production app.

    $ ng build --prod

### Step 2 - Build The Docker Image for the version 1
Run the  command to build, tagged Version 1

    $ docker image build -t my-angular-app:version1 .

### Step 3 - Build The Docker Image for the version 2
Change Directory to..

    $ cd AngularCli-Version2
    
Build the Images again, for version 2

    $ docker image build -t my-angular-app:version2 .

### Step 4 - Create a new host in your system.
    $ cd /
    $ nano etc/hosts
Add new Host, which names angular-zero.com

    127.0.1.3       angular-zero.com

### Step 5 - Initialize the Docker Swarm Mode
Go back to the main dicrectory..

    $ cd ..
    $ docker swarm init --advertise-addr <machine-ip>    
    
### Step 6 - Creating Common Network with --driver Overlay
    $ docker network create --driver overlay --attachable angular-cli-network


### Step 5 - Deploy Application into the Swarm Mode
    $ sudo docker stack deploy -c docker-compose-version-1.yml blue_stack
    $ sudo docker stack deploy -c docker-compose-version-1.yml green_stack

### Step 6 - Deploy the nginx into the stack
Nginx server to proxy pass the request from angular-zero.com to upstream server.

    $ sudo docker stack deploy -c docker-compose-nginx.yml nginx_stack

### Step 7 - First, Updating green_stack with latest version.
At any time, only one of the environments is live, with the live environment serving all production traffic. For this example, Blue is currently live and Green is updating.

    $ sudo docker stack deploy -c docker-compose-version-2.yml green_stack
    
### Step 8 - Updating blue_stack with latest version.   
At this time, Green is currently live and blue is updating.

    $ sudo docker stack deploy -c docker-compose-version-2.yml blue_stack
