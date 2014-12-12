Exercices to play with btrfs
============================

# Reference

[Howtoforge](http://www.howtoforge.com/a-beginners-guide-to-btrfs)

# Exercises

## Create filesystem
* RAID1 metadata + RAID0 data

`$ sudo mkfs.btrfs /dev/sdb /dev/sdc /dev/sdd` 

* or RAID0 metadata + RAID0 data

`$ sudo mkfs.btrfs -m raid0 /dev/sdb /dev/sdc /dev/sdd`

* or RAID1 metadata + RAID1 data

`$ sudo mkfs.btrfs -d raid1 /dev/sdb /dev/sdc /dev/sdd`

* for RAID10 => `-m raid10 -d raid10`

## Check filesystem
```
$ sudo btrfs filesystem show /dev/sdb
$ sudo btrfs filesystem show /dev/sdc
$ sudo btrfs filesystem show /dev/sdd
```

## Mount filesystem
`$ sudo mount /dev/sdb /mnt`

* equiv `$ sudo mount /dev/sdc /mnt`
* equiv `$ sudo mount /dev/sdd /mnt`

`$ sudo btrfs filesystem df /mnt`

## Compression
`$ sudo mount -o compress=lzo /dev/sdb /mnt`

## Resize
* ex reduce 2g

`$ sudo btrfs filesystem resize -1g /mnt`

* ex use max capacity

`$ sudo btrfs filesystem resize max /mnt`

## Add a device
`$ sudo btrfs device add /dev/sde /mnt`

`$ sudo btrfs filesystem show /dev/sdb`

* Need to rebalance data with the new device (not RAID0)

`$ sudo btrfs filesystem balance /mnt`

or

`$ sudo btrfs balance start /mnt`

* Check rebalance

`$ sudo btrfs filesystem show /dev/sdb`

## Delete a device

`$ sudo btrfs device delete /dev/sdc /mnt`

## Change RAID level (need newer btrfs-tools than 0.19)

`$ sudo btrfs balance start -dconvert=raid1 -mconvert=raid1 /mnt`

## Creating Subvolumes

`$ sudo btrfs subvolume create /mnt/sv1`

`$ sudo btrfs subvolume list /mnt`

## Mount subvolumes

`$ sudo mount -o subvolid=266 /dev/sdb /mnt`

## Deleting subvolumes

`$ sudo btrfs subvolume delete /mnt/sv1`

`$ sudo btrfs subvolume list /mnt`

## Creating snapshot

* test files

```
$ sudo touch /mnt/sv1/test1 /mnt/sv1/test2

$ sudo btrfs subvolume snapshot /mnt/sv1 /mnt/sv1_snapshot

$ ls -l /mnt/sv1_snapshot
```

## Files snapshot

`$ cp --reflink /mnt/sv1/test1 /mnt/sv1/test3`

## Defragmentation

`$ sudo btrfs filesystem defrag /mnt`

## Convert an ext3/ext4 FS to BTRFS

```
$ sudo umount /mnt

$ sudo fsck -f /dev/sdb1

$ sudo btrfs-convert /dev/sdb1

$ sudo mount /dev/sdb1 /mnt

$ sudo btrfs subvolume list /mnt
```

* if no need to do a rollback later

`sudo btrfs subvolume delete /mnt/ext2_saved`

## Rollback to ext3/ext4

```
$ sudo umount /mnt

$ sudo btrfs-convert -r /dev/sdb1

$ sudo mount /dev/sdb1 /mnt
```
