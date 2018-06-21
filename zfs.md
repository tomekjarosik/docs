[simple-zfs-introduction](https://tutorials.ubuntu.com/tutorial/setup-zfs-storage-pool?_ga=2.175880293.631315169.1525465131-299326657.1521497219#0)
[wiki-kernel-zfs](https://wiki.ubuntu.com/Kernel/Reference/ZFS?_ga=2.251467201.631315169.1525465131-299326657.1521497219)
[how-to-set-a-quota-on-zfs](https://docs.oracle.com/cd/E23823_01/html/819-5461/gazvb.html)
[samba-on-zfs](http://www.supermaru.com/2017/05/ubuntu-zfs-samba-share/)
https://blog.programster.org/zfs-create-disk-pools

[cheat-sheet](https://www.thegeekdiary.com/solaris-zfs-command-line-reference-cheat-sheet/)
```
# create a zfs pool:
sudo zpool create mypool /dev/sdb /dev/sdc 
# and volumes inside it, mounted by default in /mypool/
# you can custoum mount with: `zfs create -o mountpoint=/export/zfs tank/home`
sudo zfs create mypool/tmp
sudo zfs create mypool/projects
# set a quota:
zfs set quota=20g mypool/projects

# list volumes and pools
$ zfs list
NAME                                                                                USED  AVAIL  REFER  MOUNTPOINT
ssd850evo                                                                          2.06G   447G    24K  none
ssd850evo/containers                                                                870M   447G    24K  none
ssd850evo/containers/dev-redis                                                      225M   447G   839M  /var/lib/lxd/storage-pools/ssd850evo/containers/dev-redis
ssd850evo/containers/rabbitmq                                                       628M   447G  1.18G  /var/lib/lxd/storage-pools/ssd850evo/containers/rabbitmq
ssd850evo/containers/ssh-jump-host                                                 16.7M   447G   642M  /var/lib/lxd/storage-pools/ssd850evo/containers/ssh-jump-host
ssd850evo/custom                                                                     24K   447G    24K  none
ssd850evo/deleted                                                                    24K   447G    24K  none
ssd850evo/images                                                                   1.21G   447G    24K  none
ssd850evo/images/09f5b95034ec4b0c1a59258557d32e3a11800c7cb61d34f2dfd5883c078931fd   603M   447G   603M  none
ssd850evo/images/b36ec647e374da4816104a98807633a2cc387488083d3776557081c4d0333618   633M   447G   633M  none
ssd850evo/snapshots                                                                  24K   447G    24K  none

```

Backup and restore:
[oracle tutorial](https://pavelanni.github.io/oracle_solaris_11_labs/zfs/zfs_backup/)
```
# backup:
zfs snapshot rpool/data@backup-`date +%Y-%m-%d`
zfs send rpool/data@backup-2016-03-28 | zfs recv -o compression=lz4 backuppool/backup
# and restore:
zfs send backuppool/backup@backup-2016-03-28 | zfs recv -o compression=lz4 -o mountpoint=/data rpool/data
```
Or to a file:
```
# backup:
zfs send rpool/data@backup-2016-03-28 > backup
# and restore:
zfs recv -o compression=lz4 -o mountpoint=/data rpool/data < backup
```

ARC:
http://www.c0t0d0s0.org/archives/5329-Some-insight-into-the-read-cache-of-ZFS-or-The-ARC.html



