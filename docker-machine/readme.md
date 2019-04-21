## Steps for Installing and running a Docker-machine

1. Install *docker-machine* by command

    ```
    base=https://github.com/docker/machine/releases/download/v0.14.0 &&
    curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine &&
    sudo install /tmp/docker-machine /usr/local/bin/docker-machine
    ```

2. Confirm Installation by command 

    `docker-machine version`

    It will give output like : 
    > docker-machine version 0.14.0, build 9371605

3. Create a docker-machine using VM driver for local installation (We can use different driver based on host we need to install, here we are using Virtual Box driver) where testVM is name of vm we are creating:

    `docker-machine create --driver=virtualbox testVM`

4. Now check if any *docker-machines* are available in system

    `docker-machine ls`

5. Get the environment commands for your new VM.
   As noted in the output of the docker-machine create command, you need to tell Docker to talk to the new machine. You can do this with the docker-machine env command.

    `docker-machine env default`

    will give output like :

    ```
    export DOCKER_TLS_VERIFY="1"
    export DOCKER_HOST="tcp://172.16.62.130:2376"
    export DOCKER_CERT_PATH="/Users/<yourusername>/.docker/machine/machines/default"
    export DOCKER_MACHINE_NAME="default"
    ```

6. Run this command to configure and connect your shell to new VM:

    `eval "$(docker-machine env testVM)"`

7. Get the IP of docker-machine VM :

    `docker-machine ip testVM`

    will give VM IP Address :

    `192.168.99.100`

8. Start and stop VM using commands :

    `docker-machine start testVM`

    `docker-machine stop testVM`

9. To login to VM shell , enter command:

    `cocker-machine ssh testVM`

> ## Note: ### whenever you restart vm run it in same sequence like test1 before , test 2 after so they have same ip address as they are alloted dynamically

