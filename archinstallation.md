# Arch Linux installation guide

## Set the console keyboard layout and font

```bash
loadkeys de
```

## Partition the disks

List your disks with

```bash
lsblk
```

or 

```bash
fdisk -l
```

Use fdisk or cfdisk to modify partition tables. For example:

```bash
cfdisk /dev/the_disk_to_be_partitioned
```

If you want to create a new empty gpt partition table on your disk (which wipes all your data on the disk), perform before these commands:

```bash
fdisk /dev/the_disk_to_be_partitioned
g
w
```

## Partition Layout

| Mount point | Size                                                                              | Type             |
| ----------- | --------------------------------------------------------------------------------- | ---------------- |
| /boot       | At least 512 MiB. If multiple kernels will be installed, then no less than 1 GiB. | EFI System       |
| SWAP        | 16 GB (2 x RAM)                                                                   | Linux swap       |
| /           | 1/3 of free disk space                                                            | Linux filesystem |
| /home       | 2/3 of free disk space                                                            | Linux filesystem |

## Format the partitions

To create an Ext4 file system on `/dev/*root_partition*` and `/dev/*home_partition*`

```bash
mkfs.ext4 /dev/root_partition
```

swap-fs for swap-partition

```bash
mkswap /dev/swap_partition
```

fs for EFI system partition

```bash
mkfs.fat -F 32 /dev/efi_system_partition
```

## Mount the file systems

```bash
mount /dev/root_partition /mnt
```

```bash
mount --mkdir /dev/efi_system_partition /mnt/boot
```

```bash
mount --mkdir /dev/home_partition /mnt/home
```

```bash
swapon /dev/swap_partition
```

## Installation

```bash
pacstrap -K /mnt base linux linux-firmware
```

If you want to install the lts kernel, install `linux-lts` instead of `linux`

## Configure the system

Generate an fstab file (use -U or -L to define by UUID or labels, respectively):

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

Change root into the new system:

```bash
arch-chroot /mnt
```

Time zone

```bash
ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime
```

```bash
hwclock --systohc
```

### Localization

Install vim

```bash
pacman -S vim
```

Uncomment needed locales

```bash
vim /etc/locale.gen
en_US.UTF-8 UTF-8
```

Generate the locales

```bash
locale-gen
```

Set LANG variable

```bash
vim /etc/locale.conf
LANG=en_US.UTF-8
```

Set keyboard layout

```bash
vim /etc/vconsole.conf
KEYMAP=de-latin1
```

### Network configuration

Create the hostname file:

```bash
vim /etc/hostname
arch
```

Install NetworkManager

```bash
pacman -S networkmanager
```

Enable NetworkManager

```bash
systemctl enable NetworkManager
```

### Initramfs

```bash
mkinitcpio -P
```

## Usermanagement

Set the root password:

```bash
passwd
```

Install sudo package

```bash
pacman -S sudo
```

Create a new sudo user

```bash
useradd -m -G wheel -s /bin/bash user
```

The -m option creates a home directory for the user /home/user.

The -G option adds the user to an additional group. In this case, the user is being added to the wheel group.

The -s option specifies the default login shell. In this case, we are assigning bash shell denoted by /bin/bash.

Set password for the new user:

```bash
passwd user
```

Open the sudoers file and uncomment following line do enable the sudo rights for the wheel group

```bash
vim /etc/sudoers
 %wheel ALL=(ALL:ALL) ALL
```

Close the file with `:wq!` because it's readonly

## Bootloader

Install the following packages

```bash
pacman -S efibootmgr grub
```

Then execute following grub command

```bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB_arch
```

Enable OS-Prober by uncommenting following file

```bash
vim /etc/default/grub
GRUB_DISABLE_OS_PROBER=false
```

Generate the main configuration file

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

## Reboot

```bash
exit
```

```bash
umount -R /mnt
```

```bash
reboot
```

# Display Manager

## greetd

```bash
pacman -S greetd greetd-tuigreet 
```

```bash
/etc/greetd/config.toml
command = "tuigreet --cmd sway"
```

```bash
sudo systemctl enable greetd
```

## sddm

```bash
pacman -S sddm
```

```bash
sudo systemctl enable sddm
```

# Desktop

[GitHub - zimsneexh/zws-setup-sway: Sway, zsh and other utilities config files.](https://github.com/zimsneexh/zws-setup-sway/)
