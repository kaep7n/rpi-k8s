
## mount

https://pimylifeup.com/raspberry-pi-mount-usb-drive/

```
sudo blkid /dev/sda1
0B568FF27D5CC25F

sudo apt install exfat-fuse
sudo apt install exfat-utils

sudo mkdir -p /mnt/nfs
sudo chown -R kaeptn:users /mnt/nfs

sudo nano /etc/fstab
UUID=0B568FF27D5CC25F /mnt/nfs ntfs defaults,auto,users,rw,nofail,noatime 0 0

sudo umount /dev/sda1
sudo mount -a
sudo reboot

```

## nfs-server

https://pimylifeup.com/raspberry-pi-nfs/

```

sudo apt-get install nfs-kernel-server -y

sudo find /mnt/nfs/ -type d -exec chmod 755 {} \;
sudo find /mnt/nfs/ -type f -exec chmod 644 {} \;

id kaeptn

sudo nano /etc/exports
/mnt/nfs *(rw,all_squash,insecure,async,no_subtree_check,anonuid=1000,anongid=100)

sudo exportfs -ra

```