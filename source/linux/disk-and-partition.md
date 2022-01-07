# Disk and Partition Management

## Useful Commands
- get disk UUID (for fstab): `blkid`
- file check: `e2fsck -vf /dev/<disk>`

## fstab

## Mount
- mount disk: `mount /dev/sdb1 /mnt`

## Partitioning

## dd
- https://wiki.archlinux.org/title/Dd

## LVM
- see https://wiki.archlinux.org/title/LVM
- see https://wiki.archlinux.org/title/Dm-crypt/Encrypting_an_entire_system#LVM_on_LUKS

### Creation Commands
- create the pv and the vg in a primary partition and not a bare disk
- create physical volume: `pvcreate /dev/<partition>`
- create volume group: `vgcreate <volume_group_name> /dev/<partition>` (same `<partition>` as for the physical volume)
- create logical volume with fixed size: `lvcreate -L <size>G <volume_group_name> -n <logical_volume_name>`
- create logical volume with percentage of free size (100 for remaining size): `lvcreate -l <site_in_percent>%FREE <volume_group_name> -n <logical_volume_name>`
- format with ext4: `mkfs.ext4 /dev/<volume_group_name>/<logical_volume_name>`

### List Commands
- list physical volumes: `pvs`
- list volume groups: `vgs`
- list logical volumes: `lvs`
- details about volume groups (including allocated and remaining space): `vgdisplay`
- details about logical volumes: `lvdisplay`

### Snapshots
- https://wiki.archlinux.org/title/LVM#Snapshots
- create snapshot: `lvcreate --size <size>G --snapshot --name <snapshot_name> /dev/<volume_group_name>/<logical_volume_name>`

### Resizing
- https://wiki.archlinux.org/title/LVM#Resize_physical_volume
- https://wiki.archlinux.org/title/Resizing_LVM-on-LUKS