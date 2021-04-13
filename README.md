# arch_linux_install

ping -c 3 archlinux.org
fdisk -l
cfdisk /dev/sda
mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda2
mkfs.ext4 /dev/sda3
mount /dev/sda2 /mnt
mkdir /mnt/home
mount /dev/sda3 /mnt/home
pacstrap /mnt base linux linux-firmware nano
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt /bin/bash
ln -sf /usr/share/zoneinfo/Europe/Kiev /etc/localtime
hwclock --systohc --utc
echo username > /etc/hostname
nano /etc/hosts
127.0.0.1 localhost.localdomain username
pacman -S networkmanager
systemctl enable NetworkManager
passwd
pacman -S grub efibootmgr
mkdir /boot/efi
mount /dev/sda1 /boot/efi/
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB --removable
grub-mkconfig -o /boot/grub/grub.cfg
nano /etc/pacman.conf
#[multilib]
#Include = /etc/pacman.d/mirrorlist
exit
umount -R /mnt
reboot
root
pass
useradd -m -g users -G wheel -s /bin/bash username
passwd username
pass
pacman -S sudo
EDITOR=nano visudo
exit
username
pass
sudo pacman -S pulseaudio pulseaudio-alsa xfce4-pulseaudio-plugin xorg xorg-xinit xorg-server xfce4 lightdm lightdm-gtk-greeter
Установим драйвер для видеокарты:
Intel: sudo pacman -S xf86-video-intel
Nvidia: sudo pacman -S xf86-video-nouveau
AMD: sudo pacman -S xf86-video-ati
Виртуальная машина: sudo pacman -S xf86-video-vesa
echo "exec startxfce4" > ~/.xinitrc
sudo systemctl enable lightdm
startx
===========================================================================================
РУСИФИКАЦИЯ ТЕРМИНАЛА

sudo nano /etc/vconsole.conf  (Правим настройки клавиатуры и шрифтов)
KEYMAP=ru
FONT=cyr-sun16

sudo nano /etc/locale.gen
Пишем en_US.UTF-8 UTF-8 и ru_RU.UTF-8 UTF-8
sudo locale-gen
===========================================================================================
sudo pacman -S ttf-liberation ttf-dejavu opendesktop-fonts ttf-bitstream-vera ttf-arphic-ukai ttf-arphic-uming ttf-hanazono
===========================================================================================
РУСИФИКАЦИЯ СИСТЕМЫ

sudo nano /etc/locale.conf
Пишем: LANG=ru_RU.UTF-8 (Ctrl+X и Y и Enter) перезагрузка и система на русском.
===========================================================================================
УСТАНОВКА YAOURT

sudo pacman -S --needed base-devel git wget yajl
cd /tmp
git clone https://aur.archlinux.org/package-query.git
cd package-query/
makepkg -si
cd ..
git clone https://aur.archlinux.org/yaourt.git
cd yaourt/
makepkg -si
cd ..
sudo rm -dR yaourt/ package-query/
===========================================================================================
dd bs=4M if=/path/to/archlinux.iso of=/dev/sdb status=progress && sync
===========================================================================================
