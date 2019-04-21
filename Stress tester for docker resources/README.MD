# For Stress testing we can use these preconfigured Docker images to stress test again System resources

## Docker Killer Image

This image can be used to stress test docker with varying
- Cpu usage
- Memory Usage
- Network usage
- Kernel

### Particular Tests

#### cpubomb

It will 100% utilize all CPUs.
```
docker run \
    --rm -ti \
    --privileged \
    -v /:/rootfs \
    --oom-kill-disable \
    monitoringartist/docker-killer \
    cpubomb
```

#### membomb
It will exhaust host memory and swap space.
```
docker run \
    --rm -ti \
    --privileged \
    -v /:/rootfs \
    --oom-kill-disable \
    monitoringartist/docker-killer \
    membomb
```
####  netbomb
It will utilize your internet connectivity by using UDP stream.

```
docker run \
    --rm -ti \
    --privileged \
    -v /:/rootfs \
    --oom-kill-disable \
    monitoringartist/docker-killer \
    netbomb
```

#### forkbomb
It will execute number of forks.
```
docker run \
    --rm -ti \
    --privileged \
    -v /:/rootfs \
    --oom-kill-disable \
    monitoringartist/docker-killer \
    forkbomb
```
Solution: Use your standard OS number of processes limitations: number of processes per user and use custom user with defined limit in the container.

#### kernelpanic
It will generate kernel panic.

```
docker run \
    --rm -ti \
    --privileged \
    -v /:/rootfs \
    --oom-kill-disable \
    monitoringartist/docker-killer \
    kernelpanic
```
Solution: Don't run any Docker images, which can cause kernel panic.

#### die
Exit container with exit code 1.

```
docker run \
    --rm -ti \
    --privileged \
    -v /:/rootfs \
    --oom-kill-disable \
    monitoringartist/docker-killer \
    die
```


### Progrium/DockerStress
It is a docker project that you can use to stress test against Cpu, Memory, I/O and network.

`docker run --rm -it progrium/stress --cpu 2 --io 1 --vm 2 --vm-bytes 128M --timeout 10s`