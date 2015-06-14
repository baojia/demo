demo
====

loadkeys us
cfdisk /dev/sda
mkfs.ext4 /dev/sda1
mkfs.ext4 /dev/sda3
mkswap /dev/sda2
swapon /dev/sda2
mount /dev/sda3 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot

genfstab /mnt >> /mnt/etc/fstab

nano /etc/pacman.d/mirrorlist
pacstrap /mnt base base-devel

arch-chroot /mnt

nano /etc/locale.gen
locale-gen

echo arch >> /etc/hostname
ln -s /usr/share/zoneinfo/Asia/Chongqing /etc/localtime

pacman -S grub os-prober

grub-install /dev/sda
mkinitcpio -p linux
grub-config /boot/grub/grub.cfg

umount /mnt/boot
umount /mnt
reboot

dhcpcd
systemctl enable dhcpcd

nano /etc/pacman.conf
pacman -Syy
pacman -Su

pacman -S alsa-utils
alsamixer
speaker-test -c 2

pacman -S xorg-server xorg-server-utils xorg-xinit

pacman -S virtualbox-guest-utils
modprobe -a vboxguest vboxsf vboxvideo
nano /etc/modules.d/virtualbox.conf (vboxguest vboxsf vboxvideo)

pacman -S xorg-xwm xclock xterm

nano /etc/sudoers
useradd -m -g users,sudo baojia

pacman -S ttf-dejavu

pacman -S enlightenment
nano .xinitrc
exec enlightenment_start
