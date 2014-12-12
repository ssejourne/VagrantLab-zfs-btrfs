Exercices to play with zfs
==========================

# References

* https://pthree.org/2012/04/17/install-zfs-on-debian-gnulinux/
* https://flux.org.uk/tech/2007/03/zfs_tutorial_1.html
* https://flux.org.uk/tech/2007/03/zfs_tutorial_2.html

# Exercises

## Create RAID-Z disk array

```
$ sudo zpool create -f datastore raidz /dev/sdb /dev/sdc /dev/sdd

$ sudo zpool list

$ sudo zpool status datastore
```

* RAIDZ = 1 parity disk (~RAID5)
* RAIDZ2 = 2 parity disks (~RAID6)
* RAIDZ3 = 3 parity disks

### Mirror of 4 disks :

`$ sudo zpool create datastore mirror /dev/sdb /dev/sdc /dev/sdd /dev/sde`

### RAID 10

`$ sudo zpool create datastore mirror /dev/sdb /dev/sdc mirror /dev/sdd /dev/sde`

## Destroy disk array

`$ sudo zpool destroy -f datastore`

## Add device to a pool

```
$ sudo zpool create datastore /dev/sdb /dev/sdc /dev/sdd
$ sudo zpool list
NAME        SIZE  ALLOC   FREE    CAP  DEDUP  HEALTH  ALTROOT
datastore  5.95G    94K  5.95G     0%  1.00x  ONLINE  -

$ sudo zpool add datastore /dev/sde
$ sudo zpool list
NAME        SIZE  ALLOC   FREE    CAP  DEDUP  HEALTH  ALTROOT
datastore  7.94G   200K  7.94G     0%  1.00x  ONLINE  -
```

## Create ZFS dataset

```
$ sudo zfs create -o mountpoint=/mnt/binaries datastore/binaries
$ sudo zfs create -o mountpoint=/mnt/homes datastore/homes
$ sudo zfs create -o mountpoint=/mnt/backups datastore/backups

$ sudo zfs list
```

## Dataset compression

```
$ sudo zfs get compression datastore/homes

$ sudo zfs set compression=on datastore/homes
```

### Change compression from gzip-6

```
$ sudo zfs set compression=gzip-9 datastore/homes

$ sudo zfs set compression=lzjb datastore/homes
```

### Remove compression

`$ sudo zfs set compression=off datastore/homes`

## Dataset encryption (pas sous linux)

```
$ sudo zfs create -o encryption=on mountpoint=/mnt/homes datastore/homes
$ sudo zfs get encryption datastore/homes
```

## Get dataset properties

`$ sudo zfs get all datastore/homes`

## Get perf

`$ sudo zpool iostat -v datastore`

## Quota & reservation

```
$ sudo zfs set quota=1G datastore/homes

$ sudo zfs get quota datastore datastore/homes

$ sudo zfs set reservation=1G datastore/binaries

$ sudo zfs get -r reservation datastore
```

## Deduplication (/!\RAM)

```
$ sudo zfs set dedup=on datastore/homes
$ sudo zfs get dedup datastore/homes
NAME             PROPERTY  VALUE          SOURCE
datastore/homes  dedup     on             local
```
