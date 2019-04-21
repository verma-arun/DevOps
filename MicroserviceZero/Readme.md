# Blue Green Zero Down Time Deployment Using Nginx in Swarm Mode.
[![N|Solid](http://depcon.containx.io/assets/img/docs/blue-green-deployment-sets.png)](https://nodesource.com/products/nsolid)
##### Swarm mode refers to cluster management and orchestration features embedded in Docker Engine. When you initialize a new swarm (cluster) or join nodes to a swarm, the Docker Engine runs in swarm mode.
#
#### Perform the following steps to start :
#### Step 1 - Initialize the swarm mode
        $ docker swarm init --advertise-addr MANAGER_IP
You will also notice that the output mentions the docker swarm join command to use in case you want another node to join as a worker. Keep in mind that you can have a node join as a worker or as a manager. At any point in time, there is only one LEADER and the other manager nodes will be as backup in case the current LEADER opts out. 
        
#### Step 2 - Create a yml configuration to run your image
    version: '3.4'
    services:
    app:
        image: house:v1
        environment:
            - SPRING_PROFILES_ACTIVE=prod,swagger
        networks:
            - jhipster-network
        deploy:
          replicas: 2
          update_config:
            parallelism: 1
            delay: 60s
            order: start-first
          restart_policy:
            condition: on-failure
        ports:
            - 8080:8080
    networks:
        jhipster-network:
        external: true

## Configuration Details
**Application Image** - Image to running the service. v1 is the tag of the image.

        images: house:v1
**Environment** - Used to set the container inside environment. Spring_Profile for Spring Project.

        environment:
            - SPRING_PROFILES_ACTIVE=prod,swagger

**Network** - Create a Common Network to deploy all services.

        networks:
            - jhipster-network
            
**Deploy** - Specify configuration related to the deployment and running of services. This only takes effect when deploying to a swarm with docker stack deploy.            

- **replicas** - if the service is replicated (which is the default), specify the number of containers that should be running at any given time.
- **update_config** - Configures how the service should be updated. Useful for configuring rolling updates.
        - **parallelism** - The number of containers to update at a time.
        - **delay** - The time to wait between updating a group of containers.
        - **order** - Order of operations during updates. One of stop-first (old task is stopped before starting new one), or start-first (new task is started first, and the running tasks briefly overlap) (default stop-first)
- **restart_policy** - Configures if and how to restart containers when they exit.
        - **condition** - One of none, on-failure or any (default: any).
        
        deploy:
          replicas: 2
          update_config:
            parallelism: 1
            delay: 60s
            order: start-first
          restart_policy:
            condition: on-failure

#### Step 2 - Deploy stack into the swarm with green_stack
        $ docker stack deploy -c house-v1.yml green_stack

#### Step 3 - yml for run nginx
        version: "3"
        services:
          nginx:
            image: nginx
            volumes:
              - ./nginx.conf:/etc/nginx/conf.d/default.conf
            networks:
              - jhipster-network
            deploy:
              resources:
                limits:
                  cpus: "0.1"
                  memory: 50M
              restart_policy:
                condition: on-failure
            ports:
              - 80:80
            command: [nginx-debug, '-g', 'daemon off;']
        networks:
          jhipster-network:
            external: true
#### Step 4 - Deploy nginx into the swarm using same network
        $ docker stack deploy -c nginx.yml nginx_stack
#### Nginx Configurations
##### The ngx_http_upstream_module module is used to define groups of serversthat can be referenced by the proxy_pass, define a server block and proxy-pass to upstream servers.
##
- **zero_upstream** having two group of servers. One is for green deployment and one is for blue deployment using when rolling back the updates.
        - **weight** - sets the weight of the server, by default, 1.
        - **fail_timeout** - **For Health Check**, the time during which the specified number of unsuccessful attempts to communicate with the server should happen to consider the server unavailable 
        - **backup** - marks the server as a backup server. It will be passed requests when the primary servers are unavailable.
####
        upstream zero_upstream {
            server blue_stack_house-app:8080 fail_timeout=10s weight=1000;
            server green_stack_house-app:8080 fail_timeout=10s weight=100 backup;
            ip_hash;
        }
            
        server {
            listen 80;
            server_name house.com;
        
            location / {
                proxy_pass http://zero_upstream;
            }
        }
        
        server {
            listen 80;
            server_name registry.com;
        
            location / {
                proxy_pass http://registry_jhipster-registry:8761/;
            }
        }

#### Step - 5 Role Back Updates
##### First Updates the blue_stack and then updates the green_stack, when roll back updates it takes time and all the request goes to backup server "  
#
**Deploy stack into the swarm with green_stack**

    $ docker stack deploy -c house-v2.yml blue_stack
## 
**After Updated the blue stack, update the green_stack**

    $ docker stack deploy -c house-v2.yml green_stack
    
    
    
    
    
