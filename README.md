# Arch Installation Documentation | Spencer Stith

## Download & Verification
1. Download Arch isofrom [here](http://mirrors.acm.wpi.edu/archlinux/iso/2021.10.01/)
2. Download signiature from [here](https://archlinux.org/download/)
3. Verify the image using
```bash
gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig
```
(image and signiature must be in same location)

4. Put that bad boy in VMware and get going!

## Memory Management
1. Run `fdisk -l` to view drives on the computer.
2. Run `fdisk /dev/sda`
3. Create a new table: `g`
4. Create a EUFI partition: `n`
5. Keep defaults but make the size `+512MB`
6. Change type: `t`, `1`
7. Create root partition: `n`
8. All defaults
9. Write changes: `w`
10. Create filesystem for EUFI: `mkfs.fat -F32 /dev/sda1`
11. Create filesystem for root: `mkfs.ext4 /dev/sda2`
12. Mount: `mount /dev/sda2 /mnt`

## Installation & Configuration
1. Install necessary packagets: `pacstrap /mnt base linux linux-firmware`
3. Generate the fstab: `genfstab -U /mnt >> /mnt/etc/fstab`
4. Enter the disk as root: `arch-chroot /mnt`
5. Install nano: `pacman -Sy nano`
6. Set the timezone: `ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime` then `hwclock --systohc`
7. Generate locales: `locale-gen` then `echo en_US.UTF-8 > /etc/locale.conf`
8. Set the hostname: `echo SCSArch > /etc/hostname`
9. Add hosts: `nano /etc/hosts`
10. Put:

```
127.0.0.1       localhost
::1             localhost
127.0.0.1       SCSArch
```
Ctrl+X, then Y to exit/save

10. Change root password: `passwd`

## Bootloader
1. Install GRUB: `pacman -S grub efibootmgr`
2. Create partition: `mkdir /boot/efi`
3. Mount that bad boy: `mount /dev/sda1 /boot/efi`
4. Install: `grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot/efi`
5. Configuration: `grub-mkconfig -o /boot/grub/grub.cfg`

## GNOME
1. 
