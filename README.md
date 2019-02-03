## go-docker-swarm

+ **Docker**: Create a single container.
+ **Docker Compose**: Manager and Orchestrate multiple containers on a single host.
+ **Docker Machine**: Create and manage VMs running docker daemon.
+ **Docker Swarm**: A Cluster of docker nodes.
+ **Docker Stack**: Deploy, Manage, and Orchestrate multiple containers on a swarm cluster.

## Running the app on a single host using docker-compose

```bash
$ docker-compose up
```

```bash
$ curl http://localhost:8080/
3f741654cee0: Welcome! Please hit the `/qod` API to get the quote of the day.

$ curl http://localhost:8080/qod
3f741654cee0: If I work as hard as I can, I wonder how much I can do in a day?
```

## Running the app on a swarm cluster using docker-stack

Read the tutorial: [Container Orchestration with Docker Machine, Docker Stack, and Swarm](https://www.callicoder.com/docker-machine-swarm-stack-golang-example/)
