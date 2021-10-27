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
1. Install necessary packagets: `pacstrap /mnt base linux linux-firmware nano`
3. Generate the fstab: `genfstab -U /mnt >> /mnt/etc/fstab`
4. Enter the disk as root: `arch-chroot /mnt`
5. Set the timezone: `ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime` then `hwclock --systohc`
6. Generate/set locales: `sed -i 's/#en_US.UTF-8/en_US.UTF-8/g' /etc/locale.gen` then `locale-gen`
7. Set the hostname: `echo SCSArch > /etc/hostname`
8. Add hosts: `nano /etc/hosts`
9. Put:

```
127.0.0.1       localhost
::1             localhost
127.0.0.1       SCSArch
```
Ctrl+X, then Y to exit/save

10. Change root password: `passwd`

## Bootloader
1. Install GRUB and other tools: `pacman -S grub efibootmgr dosfstools os-prober mtools linux-headers linux-lts linux-lts-headers`
2. Create partition: `mkdir /boot/EFI`
3. Mount that bad boy: `mount /dev/sda1 /boot/EFI`
4. Install: `grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck`
5. Set GRUB's locale: `cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo`
6. Configuration: `grub-mkconfig -o /boot/grub/grub.cfg`
7. `exit`, `umount -a`, `reboo t`

## GNOME
1. Install X environment: `pacman -S xorg`
2. Install GNOME: `pacman -S gnome`
3. Enable the display manager and NetworkManager:

```bash
systemctl start gdm.service
systemctl enable gdm.service
systemctl enable NetowrkManager.service
```
4. Exit chroot: `exit `
5. Reboot: `sudo reboot`
