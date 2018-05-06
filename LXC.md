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
lxc launch ubuntu:18.04/amd64 -s my-zfs-pool
```

### Storage
`lxc storage list` and `lxc storage create my-storage-pool zfs`.
More here:
https://lxd.readthedocs.io/en/latest/storage/



Resources
====
[lxd introduction](https://stgraber.org/2016/03/11/lxd-2-0-introduction-to-lxd-112/) )
[ubuntu-lxd](https://powersj.github.io/post/ubuntu-lxd/)
[getting started LXD](https://linuxcontainers.org/lxd/getting-started-cli/)
[lxd storage](https://insights.ubuntu.com/2017/07/12/storage-management-in-lxd-2-15)





