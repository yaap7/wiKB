# Mount a VDI disk image

Pour monter un disque d'une machine virtuelle (VM) créée avec VirtualBox, il est possible d'utiliser qemu-nbd :

``` bash
sudo aptitude install qemu-kvm
sudo modprobe nbd
sudo qemu-nbd -c /dev/nbd0 image.vdi
sudo mkdir /media/disk_vm
sudo mount /dev/nbd0p1 /media/disk_vm
((( do stuff )))
sudo umount /media/disk_vm
sudo rmdir /media/disk_vm
sudo qemu-nbd -d /dev/nbd0
sudo rmmod nbd
```

[source](https://bethesignal.org/blog/2011/01/05/how-to-mount-virtualbox-vdi-image/)
