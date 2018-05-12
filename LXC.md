# docs

LXC containers
====

List of available containers:
https://us.images.linuxcontainers.org/

How to start a new container:
```lxc launch ubuntu:18.04/amd64 my-container```

Then:
`lxc list` and 
```
lxc exec my-container bash
```
or start container on a specific storage
```
lxc launch ubuntu:18.04/amd64 my-redis -s my-zfs-pool
```
start bash inside it:
```
lxc exec my-redis bash
```

### Storage
```
lxc storage list
lxc storage create my-storage-pool zfs
lxc storage info <your-pool-name>
```

More here:
https://lxd.readthedocs.io/en/latest/storage/


TODO verify this:
```
lxc storage volume create <pool-name> <volume-name>
lxc storage volume attach <pool-name> <volume-name> <container-name> <device-name> path=</some/path/in/the/container>
```


Resources
====
[lxd introduction](https://stgraber.org/2016/03/11/lxd-2-0-introduction-to-lxd-112/) )
[ubuntu-lxd](https://powersj.github.io/post/ubuntu-lxd/)
[getting started LXD](https://linuxcontainers.org/lxd/getting-started-cli/)
[lxd storage](https://insights.ubuntu.com/2017/07/12/storage-management-in-lxd-2-15)
[lxd cheat-sheet](https://www.jamescoyle.net/cheat-sheets/2540-lxc-2-x-lxd-cheat-sheet)





