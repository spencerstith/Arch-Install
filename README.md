# Arch Installation Documentation | Spencer Stith

## ✅ Download & Verification ✅
1. Download Arch isofrom [here](http://mirrors.acm.wpi.edu/archlinux/iso/2021.10.01/)
2. Download signiature from [here](https://archlinux.org/download/)
3. Verify the image using
```bash
gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig
```
(image and signiature must be in same location)

4. Put that bad boy in VMware and get going!
5. Make sure you are using EUFI
6. Allocate about 20 GB of storage

*Notes/Struggles*
* Didn't allocate enough storage at first so I had to do some repartitioning later on and it sucked a lot

## 🧠 Memory Management 🧠

### 🍰 Disk Partitioning 🍰
1. Open cgdisk: `cgdisk`
2. Go to new from `free space`
3. -> Default first sector
4. -> Size: `512M`
5. -> Hex: ef00
6. -> Name: `boot`
7. Go to new from `free space`
8. -> Default first sector
9. -> Size: `500M`
10. -> Hex: 8200
11. -> Name: `swap`
12. Go to new from `free space`
13. -> Default first sector
14. -> Default last sector
15. -> Default size
16. -> Hex: `8300`
17. -> Name: `arch`
18. Write / Quit
19. Verify with `lsblk`

*Notes/Struggles*
* Originally used `fdisk` but it was not very intuitive/easy to use
* Didn't originally use swap but I was running out of memory for somer parts of the installation, so I decided to use one

### 📖 Formatting 📖
1. Format disks:
2. -> `mkfs.fat -F32 /dev/sda1`
3. -> `mkswap /dev/sda2`
4. -> `mkfs.ext4 /dev/sda3`
### 🐎 Mount partitions 🐎
1. -> `mount /dev/sda3 /mnt`
2. -> `swapon /dev/sda2`
3. -> `mkdir -p /mnt/boot`
4. -> `mount /dev/sda1 /mnt/boot`


## 📲 Installation & Configuration 📲
1. Install essential packages: `pacstrap /mnt base linux linux-firmware base-devel nano`
2. F🔪 (fstab): `genfstab -U /mnt >> /mnt/etc/fstab`
3. Enter the installation: `arch-chroot /mnt`

### 🌎 Time/Locale 🌎
1. Set the timezone: `ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime`
2. Generate clock settings: `hwclock --systohc`
3. Generate locale settings: `nano /etc/locale.gen`
4. Uncomment `en_US.UTF-8`
5. Run `locale-gen`
6. Create locale configurations: `nano /locale.conf`
7. -> Add `LANG=en_US.UTF-8`

### ⚡️ Networking ⚡️
1. Create hostname file: `nano /etc/hostname`
2. -> Add `SCSArch`
3. Initramfs: `mkinitcpio -P`
4. Set root password: `passwd`

*Notes/Struggles*
* Initframs isn't necessary but I read to do it anyways to be more sure of a clean initial boot

## 🥾 Bootloader 🥾
1. Install GRUB: `pacman -Sy grub efibootmgr`
2. Install more GRUB: `grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB`
3. Create GRUB configuration: `grub-mkconfig -o /boot/grub/grub.cfg`
4. Reboot. Moment of truth 😬😭
5. -> Leave arch-chroot: `exit`
6. -> Restart: `shutdown -r now`

*Notes/Struggles*
* This section was by far the worst part
* I kept loading into the GRUB CLI and didn't know what to do
* I wasn't creating an EFI partition correctly in the beginning

## 😎 Arch Time 😎
### ⚡️ Networking ⚡️
1. Enable netowrking
2. -> `systemctl start systemd-networkd`
3. -> `systemctl start systemd-resolved`
4. Find network devices: `networkctl list`
5. Note our network adapter
6. Congigure it: `nano /etc/systemd/network/20-wired.network`

```bash
[Match]
Name=<adapter>

[Network]
DHCP=yes
```

7. Restart
8. -> `systemctl restart systemd-networkd`
9. -> `systemctl restart systemd-resolved`
10. Enable them at boot
11. -> `systemctl enable systemd-networkd`
12. -> `systemctl enable systemd-resolved`

### 👨🏻‍💻 Users 👨🏻‍💻
1. Be god and create life: `useradd -g users -G wheel,storage,power -m spencer`
2. Set the password: `passwd spencer`
3. Create Sal/Codi accounts
4. -> `useradd -g users -G wheel,storage,power -m codi`
5. -> `passwd codi`: GraceHopper1906
6. -> `useradd -g users -G wheel,storage,power -m sal`
7. -> `passwd sal`: GraceHopper1906
8. Become super user: `EDITOR=/usr/bin/nano visudo`
9. -> Uncomment `# %wheel ALL=(ALL) NOPASSWD: ALL`


## 💅🏻 Customization 💅🏻
###  GNOME
1. 
