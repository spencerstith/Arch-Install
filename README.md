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
2. Note the drive that you will partition.
3. Run `fdisk <drive_name>`
4. Create a new table with `g`
5. Create a new partition with `n`
6. Stick with all of the defaults, creating an 8 GB partition
7. Write changes with `w`
8. Create the file system with `mkfs.ext4 /dev/sda1`
9. Mount with `mount /dev/sda1 /mnt`

## Installation & Configuration
1. Install the essentials with `pacstrap /mnt base linux linux-firmware`
2. Install nano with `sudo pacman -Sy nano`
3. Generate the fstab with `genfstab -U /mnt >> /mnt/etc/fstab`
4. Enter the disk as root with `arch-chroot /mnt`
5. `exit`
6. Set the timezone with `ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime` then `hwclock --systohc`
7. Generate locales with `locale-gen` then `echo en_US.UTF-8 > /etc/locale.conf`
8. Set the hostname with `echo SCSArch > /etc/hostname`
9. Add hosts with `nano /etc/hosts`
10. Add:

```
127.0.0.1       localhost
::1             localhost
127.0.0.1       SCSArch
```
Ctrl+X, then Y to exit/save

10. Install NetworkManager with `pacman -Sy networkmanager`
11. Enable it on booth with `sudo systemctl enable NetworkManager.service`
12. Start it up with `systemctl start NetworkManager.service`
13. 
