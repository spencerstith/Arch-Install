# Arch Installation Documentation | Spencer Stith

## Download & Verification
1. Download from [here](http://mirrors.acm.wpi.edu/archlinux/iso/2021.10.01/)
2. Download signiature from [here](https://archlinux.org/download/)
3. Verify the image using
```bash
gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig
```
(image and signiature must be in same location)

4. Put that bad boy in VMware and get going! Allocate 2 GB of memory and 40 GB of storage

## Memory Management
1. Run `fdisk -l` to view drives on the computer.
2. Note the drive that you will partition.
3. Run `fdisk <drive_name>`
4. Create a new table with `g`
5. Create a new partition with `n`
6. Stick with all of the defaults, creating a 40 GB partition
7. Write changes with `w`
8. Create the file system with `mkfs.ext4 /dev/sda1`
9. Mount with `mount /dev/sda1 /mnt`

## Installation
1. Download the essentials with `pacstrap /mnt base linux linux-firmware`
2. Download vim with `pacman -Sy vim`

## Desktop Environment
1. Find drivers with 
2. Install Xorg `sudo pacman -S xorg xterm xorg-xinit`
3. Install 
