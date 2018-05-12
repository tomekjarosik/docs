[simple-zfs-introduction](https://tutorials.ubuntu.com/tutorial/setup-zfs-storage-pool?_ga=2.175880293.631315169.1525465131-299326657.1521497219#0)
[wiki-kernel-zfs](https://wiki.ubuntu.com/Kernel/Reference/ZFS?_ga=2.251467201.631315169.1525465131-299326657.1521497219)
[how-to-set-a-quota-on-zfs](https://docs.oracle.com/cd/E23823_01/html/819-5461/gazvb.html)
[samba-on-zfs](http://www.supermaru.com/2017/05/ubuntu-zfs-samba-share/)
https://blog.programster.org/zfs-create-disk-pools
```
# create a zfs pool:
sudo zpool create mypool /dev/sdb /dev/sdc 
# and volumes inside it, mounted by default in /mypool/
# you can custoum mount with: `zfs create -o mountpoint=/export/zfs tank/home`
sudo zfs create mypool/tmp
sudo zfs create mypool/projects
# set a quota:
zfs set quota=20g mypool/projects

```




