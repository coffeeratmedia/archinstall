NU nano 4.9.2                                                                                        New Buffer                                                                                         Modified

-- partioning --
fdisk /dev/[drive]
makae follow partions
 size           partion type    type number
 512MB-1GB      efi PARTION      [1]
 30GB/+         root partion    [24]
 left over      home partion    [28]
 ramx2          swap partion    [19]

[makes new gpt table]

g

[make new partion]

n

enter or give number
enter/return
+ partion size

[set partiont type]

t

[enter the partion num]
partion number

partion  type number

[format partions]
mkfs.fat -F32 [efi partion]
mkfs.ext4 [home and root]

[make swap]
mkswap [swap partion]
swapon [swap partion]

mkdir -p /mnt/home
mkdir -p /mnt/efi

mount [root partion] /mnt
mount [home partion] /mnt/home
mounr [efi partion] /mnt/efi


nano etc/pacman.d/mirrorlist
move your closes mirror tot he top

pacstrap -i /mnt base linux linux-firmware man-db man-pages texinfo mesa intel/amd-ucode xorg-server xorg-xinit lightdm light-gtk-greeter i3 alacritty nano krusader efibootmgr netctl dhcpcd base-devel

genfstab -U -L /mnt >> /mnt/etc/fstab

arch-chroot mnt/etc/efihroot /mnt

- enable network services -

systemctl enable systemd-networkd
systemctl enable systemd-resolved
systemctl enaable ntcle



- configure network -

nano /etc/systemd/network/20-wired.network



- enable network services -

systemctl enable systemd-networkd
systemctl enable systemd-resolved
systemctl enaable ntcle



- configure network -

nano /etc/systemd/network/20-wired.network

[Match]
Name=en*

[Network]
DHCP=true

- systemd-resolved -




timedatectl set-ntp true

[ create locale.conf ]
nano /etc/locale.conf
LANG=
LANUAGE=
LC_COLLATE=C
LC_TIME=

locale-gen

vconsole.conf
KEYMAP=defaults

create /etc/hostname
[hostname]

create /etc/hosts
127.0.0.1       localhost
::1             localhost
127.0.1.1       [hostname].localdomain  [hostname]

mkinitcpio -P

passwd
[set root password]

useradd -m -g users -G wheel [username]
passwd[username]
[insert password]

nano /etc/sudoers
uncomment %wheel ALL=(ALL) ALL


-- bootloader --

efistub

efibootmgr --disk /dev/sdX --part Y --create --label "Arch Linux" --loader /vmlinuz-linux --unicode 'root=PARTUUID=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX  rw initrd=\initramfs-linux.img' --verbose


---move boot -> efi -----

mv /boot/* /efi

--- systemd-bootloder ---

 bootctl --path=/efi install
nano /etc/loader/loader.conf

default  arch.conf
timeout  1
console-mode max
editor   no

mkdir -p etc/pacman.d/hook
nano /etc/pacman.d/hooks/100-systemd-boot.hook

[Trigger]
Type = Package
Operation = Upgrade
Target = systemd

[Action]
Description = Updating systemd-boot
When = PostTransaction
Exec = /usr/bin/bootctl update

nano /efi/loader/entries/arch.conf
title   Arch Linux
linux   /vmlinuz-linux
initrd  /intel-ucode.img
initrd  /initramfs-linux.img
options root="LABEL=arch_os" rw


bootctl update

nano /efi/loader/loader.conf
-----------------------------





reboot

login

fesp/loader/loader.conf
