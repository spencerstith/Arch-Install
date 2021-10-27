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
5. -> Hex: `ef00`
6. -> Name: `boot`
7. Go to new from `free space`
8. -> Default first sector
9. -> Size: `1G`
10. -> Hex: `8200`
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
6. Create locale configurations: `nano /etc/locale.conf`
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
6. Configure it: `nano /etc/systemd/network/20-wired.network`

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
1. Install: `sudo pacman -S xorg gnome gnome-extra gdm`
2. Enable the display manager: `sudo systemctl enable gdm`
3. Reboot: `reboot`

### 🌈 Command Line 🌈
1. Install ZSH (thank god): `sudo pacman -S zsh`
2. -> `zsh`
3. -> Go through all settings, choosing defaults
4. Make ZSH the default shell: `chsh -s /usr/bin/szh`
5. Color the terminal
6. -> `PROMPT='%F{green}%n%f@%F{magenta}%m%f %F{blue}%B%~%b%f %# '`
7. -> `RPROMPT='[%F{yellow}%?%f]'`
8. Install oh-my-zsh
9. -> Install git: `sudo -S pacman git`
10. -> Install oh-my-zsh

### 🌉 Gateway 🌉
1. Install ssh: `sudo pacman -S openssh`
2. ssh to the bridge: `ssh 129.244.245.21 -p53997`
3. ssh to the VM: `ssh -p53997 admin@192.168.2.39`

### ✂️ Shortcuts/aliases
1. Colorize ls output: `alias ls='ls la --color=auto'`
2. Free memory: `alias meminfo='free -m -l -t'`
3. IP address: `alias myip='curl http://ipecho.net/plain; echo'`
4. git add all: `alias gaa='git add all'`
5. git checkout master: `alias gcm='git checkout master'`
6. Show sudoers: `alias sudoers="awk -F':' '{ print $1}' /etc/passwd"`
7. Since I have oh-my-zsh now, there are some neat commands already aliased:
8. -> Most common commands: `zsh_stats`
9. -> No `cd` is ever needed

*Notes/Struggles*
* I only did the minimum on coloring because it will go away with oh-my-zsh, and I know customization is a big part of this project, so I think it will suffice.
* I was really hoping to use iTerm2 but that's only on mac

### 👀 Chrome 👀
There is a version of Google Chrome on the Arch User Repository!
Install an AUR Helper
```bash
sudo pacman -S --needed base-devel git
git clone https://aur.archlinux.org/yay-git.git
cd yay
makepkg -si
yay -S google-chrome
```


## 🚶🏻‍♂️Walkthrough video 🚶🏻‍♂️
1. Show ip: `myip`
2. User list: `cat /etc/passwd`
3. Sudoers: `sudoers`
4. Show some aliases
5. Browser -> GH pages, Harvey
6. ssh into gateway
7. Chrome is on the AUR
8. Struggles and Problems
