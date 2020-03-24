# optimize-linux-performance
steps on improving performance on linux

Start on a minimal install
Debian net install iso
https://www.debian.org/distrib/netinst

Ubuntu mini iso
https://help.ubuntu.com/community/Ins...

Enable swap
```
sudo apt install fallocate
sudo fallocate -l 2G (swapfile_location_here)
sudo chmod 600 (swapfile_location_here)
sudo mkswap (swapfile_location_here)
sudo swapon (swapfile_location_here)
sudo swapon --show
sudo free -h
sudo sysctl vm.swappiness=10
```
```
sudo nano /etc/sysctl.conf
```
type in this
```
vm.swappiness=10
```
save and exit

```
sudo nano /etc/fstab
```
then type in this
```
(swapfile_location_here) swap swap defaults 0 0
```
MATE tweak
```
disable desktop icons
in windows untick enable animations
tick do not show window content when moving windows
under window manager pick Macro (No compositor)
```
# enable lz4 compression
```
sudo -i
```
```
apt install lz4 libcompress-lz4-perl liblz4-1 liblz4-dev liblz4-tool libroslz4-dev libroslz4-1d
```
First, check that zswap is enabled for kernel build:
```
cat /boot/config-* | grep ZSWAP
```
```
nano /etc/default/grub
```
type in this into the double quotes
```
GRUB_CMDLINE_LINUX="zswap.enabled=1 zswap.compressor=lz4 zswap.max_pool_percent=50"
```
```
grub-mkconfig -o /boot/grub/grub.cfg
```
```
nano /etc/modules
```
type in this
```
# LZ4 compression support for zswap
lz4
lz4_compress
```
Load the module
```
modprobe -v lz4
```
enable lz4 itself
```
echo 1 > /sys/module/zswap/parameters/enabled
echo lz4 > /sys/module/zswap/parameters/compressor
echo lz4 >> /etc/initramfs-tools/modules
echo lz4_compress >> /etc/initramfs-tools/modules
```
Regenerate initramfs after editing /etc/modules.
```
update-initramfs -u 
```
Zswap should now be enabled on restart.
```
reboot
```
To check that zswap is enabled, check the system messages.
```
sudo dmesg | grep 'zswap.*'
```
Adjust these steps as necessary if using a newer version of GRUB or a different bootloader.

That is all.

# videos where used
[how to improve performance on mate](https://youtu.be/XVE9R0q-f9g)

[how to enable lz4 compression](https://youtu.be/gRVzNzc5QCw)
