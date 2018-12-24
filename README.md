## go-docker-swarm

+ **Docker**: Create a single container
+ **Docker Compose**: Manager and Orchestrate multiple containers on a single host
+ **Docker Machine**: Create and manage VMs running docker daemon
+ **Docker Swarm**: A Cluster of docker nodes.
+ **Docker Stack**: Deploy, Manage, and Orchestrate multiple containers on a swarm cluster

### Create VMs using docker-machine

```bash
$ docker-machine create --driver virtualbox swarm-vm1
$ docker-machine create --driver virtualbox swarm-vm2
$ docker-machine create --driver virtualbox swarm-vm3
```

### List VMs

```bash
$ docker-machine ls
NAME        ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER     ERRORS
swarm-vm1   -        virtualbox   Running   tcp://192.168.99.100:2376           v18.09.0   
swarm-vm2   -        virtualbox   Running   tcp://192.168.99.101:2376           v18.09.0   
swarm-vm3   -        virtualbox   Running   tcp://192.168.99.102:2376           v18.09.0   
```

### Designate a VM as the Swarm manager

```bash
$ docker-machine ssh swarm-vm1 "docker swarm init --advertise-addr 192.168.99.100"
Swarm initialized: current node (j5obt23bbdcvphmgncd97q5r6) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-073v8uw449vvkwngz6g0mtgivv50dbbqzt2o9f9jmwvwrkihoj-6glz90svgl4t21z1nnio0oce2 192.168.99.100:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

### Add Workers to the Swarm cluster

```bash
$ docker-machine ssh swarm-vm2 "docker swarm join --token SWMTKN-1-073v8uw449vvkwngz6g0mtgivv50dbbqzt2o9f9jmwvwrkihoj-6glz90svgl4t21z1nnio0oce2 192.168.99.100:2377"
This node joined a swarm as a worker.

$ docker-machine ssh swarm-vm3 "docker swarm join --token SWMTKN-1-073v8uw449vvkwngz6g0mtgivv50dbbqzt2o9f9jmwvwrkihoj-6glz90svgl4t21z1nnio0oce2 192.168.99.100:2377"
This node joined a swarm as a worker.
```

### List all nodes in the swarm cluster

```bash
$ docker-machine ssh swarm-vm1 "docker node ls"
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
j5obt23bbdcvphmgncd97q5r6 *   swarm-vm1           Ready               Active              Leader              18.09.0
yb7bab6suhxsfuqlwivi090ir     swarm-vm2           Ready               Active                                  18.09.0
tqmisvt9nu8v8o7yn26rh9cox     swarm-vm3           Ready               Active                                  18.09.0
```

### Configure the current shell to talk to the Docker daemon on the VM

```bash
$  docker-machine env swarm-vm1
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/callicoder/.docker/machine/machines/swarm-vm1"
export DOCKER_MACHINE_NAME="swarm-vm1"
# Run this command to configure your shell: 
# eval $(docker-machine env swarm-vm1)
```

```bash
$ eval $(docker-machine env swarm-vm1)
```

### List all VMs (check the currently active VM)

```bash
$ docker-machine ls
NAME        ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER     ERRORS
swarm-vm1   *        virtualbox   Running   tcp://192.168.99.100:2376           v18.09.0   
swarm-vm2   -        virtualbox   Running   tcp://192.168.99.101:2376           v18.09.0   
swarm-vm3   -        virtualbox   Running   tcp://192.168.99.102:2376           v18.09.0   
```

Or Get the currently active VM like so -

```bash
$ docker-machine active
swarm-vm1
```

### Deploy the app on the Swarm manager using docker stack

```bash
$ docker stack deploy -c docker-stack.yml swarm-stack
```

### List the tasks in the stack

```bash
$ docker stack ps swarm-stack
```

### Tear down the stack

```bash
$ docker stack rm swarm-stack
```