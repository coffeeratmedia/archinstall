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


