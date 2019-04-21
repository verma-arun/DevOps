![Docker ](https://www.docker.com/sites/default/files/horizontal.png "Docker ")
### Docker Deploy with zero down time
###### Deploy a new service into the stack on swarm mode and then updating with the new service in existing stack

### Steps to Work
1.  First initialize the Swarm Mode
#### docker swarm init --advertise-addr **< Machine IP Address >
2.  Create a compose file with following configurations
#### Creating a images service with replicas 5.
    	version: "3"
    	services:
      	hello-html:
			image: hello-html:latest
			deploy:
			  replicas: 5
			  resources:
				limits:
				  cpus: "0.1"
				  memory: 50M
			  restart_policy:
				condition: on-failure
			ports:
			  - 8080:80
3. Deploy the service in docker stack
#### docker stack deploy -c docker-compose.yml nginx-hello

4. Service is running with 5 replicas
###  docker service ls

5. Deploy new service in existing stack with same name
#### docker stack deploy -c docker-compose-new.yml nginx-hello
