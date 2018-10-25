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

Add new lxc storage:
```
lxc storage create test-pool zfs source=ironwolf/containers
```

More here:
https://lxd.readthedocs.io/en/latest/storage/


TODO verify this:
```
lxc storage volume create <pool-name> <volume-name>
lxc storage volume attach <pool-name> <volume-name> <container-name> <device-name> path=</some/path/in/the/container>
```


Mount shared directory to a container. TODO: verify this:
```
nano /var/lib/lxc/containername/config
lxc.mount.entry = /media/data/share share none bind,create=dir 0.0

```

Move container to another storage pool:
```
lxc publish -f <container> --alias <container> # (this will stop the container and restart it)
lxc launch local:<hash> <container-name> -s <storage-pool-of-choosing>
```

#### Network
```
lxc init ubuntu:18.04 prod-nvme-dns -s optane900p
lxc config device add prod-nvme-dns eth0 nic nictype=bridged parent=prod-br0 name=eth0
lxc config device set prod-nvme-dns eth0 ipv4.address 10.10.10.10
lxc start prod-nvme-dns
```

Resources
====
[lxd introduction](https://stgraber.org/2016/03/11/lxd-2-0-introduction-to-lxd-112/) )
[ubuntu-lxd](https://powersj.github.io/post/ubuntu-lxd/)
[getting started LXD](https://linuxcontainers.org/lxd/getting-started-cli/)
[lxd storage](https://insights.ubuntu.com/2017/07/12/storage-management-in-lxd-2-15)
[lxd cheat-sheet](https://www.jamescoyle.net/cheat-sheets/2540-lxc-2-x-lxd-cheat-sheet)
[sharing directories](https://askubuntu.com/questions/610513/how-do-i-share-a-directory-between-an-lxc-container-and-the-host)
[resource control](https://stgraber.org/2016/03/26/lxd-2-0-resource-control-412/)





