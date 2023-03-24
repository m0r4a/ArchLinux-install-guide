# ArchLinux-install-guide


### This installation is very likely to not work for you, so just ignore this part of the readme.

```bash
timedatectl set-ntp true
```

#### Creation and formatting of the partition

_being:_

_sda1: EFI partition_

_sda2: root partition_

_sda3: home partiton_

_sda4: swap partiton_

```bash
cfdisk
mkfs.ext4 /dev/sda2
mkfs.ext4 /dev/sda3
mkswap /dev/sda4
swapon /dev/sda4
```

#### Mounting partitons

```bash
mount /dev/sda2 /mnt
mkdir /mnt/home
mount /dev/sda3 /mnt/home
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
```

#### Installaton of the system

```bash
pacstrap /mnt base linux-lts linux-lts-headers linux-firmware intel-ucode neovim
genfstab -U /mnt >> /mnt/etc/fstab
```

#### Setting up the sytem 

```bash
arch-chroot /mnt
ln -sf /usr/share/zoneinfo/America/Monterrey /etc/localtime
hwclock --systohc
nvim /etc/locale.gen	# Uncomment en_US.UFT-8 UTF-8 & es_MX.UTF-8 UTF-8
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
echo "KEYMAP=us" > /etc/vconsole.conf
echo "Arch" > /etc/hostname
```

```bash
nvim /etc/hosts

127.0.0.1	localhost
::1		localhost
127.0.1.1	Arch.localhost Arch
```

``` bash
passwd
```

#### Install the bare minimum 

_Note: this are the bare minimum till "xorg"_

```bash
pacman -Sy efibootmgr networkmanager grub base-devel git bluez bluez-utils pulseaudio pulseaudio-bluetooth alsa-utils xorg qtile lightdm lightdm-gtk-greeter alacritty picom firefox
```

#### Grub install

```bash
grub-install --target=x86_64-efi --efi-directory=/boot
grub-mkconfig -o /boot/grub/grub.cfg
```

#### Enabe system services 

```bash
systemctl enable NetworkManger
systemctl enable bluetooth
systemctl enable lightdm
systemctl start sshd
```

#### User configuration & sudo configuration

``` bash
useradd -m m0r4a
passwd m0r4a
usermod -aG wheel,audio,video,storage m0r4a
chmod u+w /etc/sudoers
nvim /etc/sudoers
84j	and uncomment: %wheel ALL=(ALL:ALL) ALL
chmod u-w /etc/sudoers
```

#### Final steps

``` bash
exit
umount -R /mnt
shutdown now
```

#### My stuff

``` bash
sudo pacman -S code pavucontrol obsidian arandr neofetch scrot brightnessctl imv feh xcalib bat lsd unzip python-pip lightdm-webkit2-greeter openssh xdg-utils libnotify dunst lxappearance qt5ct
```

_With yay_

```bash
yay -S  notification-daemon 
```

_With cargo_

```bash
cargo install toipe
```
