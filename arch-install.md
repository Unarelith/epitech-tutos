# Installation ArchLinux

### Notes pour l'installation d'un Arch x86 depuis un Arch x86_64

*Note:* Pour installer un système 32 bits il faut :

- Un système ArchLinux 32 bits

ou
   
- Un système ArchLinux 64 bits avec :
  - Un pacman.conf tweaké avec i686 en architecture et sans le dépôt multilib
  - Une mirrorlist custom
  - Le paquet `archlinux32-keyring-transition` installé

## Installation

• Faire un chroot:
```sh
mount /dev/sda2 /mnt
mount /dev/sda1 /mnt/boot/efi # only for GPT
mount /dev/sda3 /mnt/home
pacstrap /mnt base base-devel wireless_tools wpa_supplicant dialog gvim git openssh
genfstab -U -p /mnt >> /mnt/etc/fstab
# Vérifier la fstab
arch-chroot /mnt
```
• Puis, sur le chroot:
```sh
echo quentin-key > /etc/hostname
echo '127.0.1.1 quentin-key.localdomain quentin-key' >> /etc/hosts
ln -sf /usr/share/zoneinfo/Europe/Paris /etc/localtime
vi /etc/locale.gen
locale-gen
echo LANG="fr_FR.UTF-8" > /etc/locale.conf
export LANG=fr_FR.UTF-8
echo KEYMAP=fr > /etc/vconsole.conf
mkinitcpio -p linux
passwd
useradd -g users -m -s /bin/bash bazin_q
passwd bazin_q
vi /etc/sudoers
chown root:root /
chmod 755 /
vim /etc/systemd/logind.conf # HandlePowerKey=suspend
```

• Dans le cas d’un système 32 bits :

- Modifier Architecture=i686 dans `pacman.conf`
```sh
pacman -S archlinux32-keyring-transition
```

• Installer le bootloader:
```sh
pacman -S grub os-prober
```
   - Only for MBR:
```sh
grub-install --target=i386-pc --no-floppy --recheck /dev/sda
```
   - Only for GPT (EFI):
```sh
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=archlinux --recheck
```

   - Reload grub config:
```sh
grub-mkconfig -o /boot/grub/grub.cfg
```

• Installer X:
```sh
pacman -S xorg-server xorg-xinit
pacman -S xf86-video-intel
# fichiers de config clavier / carte graphique
pacman -S xorg-fonts-type1 ttf-dejavu artwiz-fonts font-bh-ttf \
          font-bitstream-speedo gsfonts sdl_ttf ttf-bitstream-vera \
          ttf-cheapskate ttf-liberation \
          ttf-freefont ttf-arphic-uming ttf-baekmuk # Polices pour sites multilingue
```
• Fichier de config pour le clavier et la carte intel dans /etc/X11/xorg.conf.d/:

10-keyboard.conf:
```xf86conf
Section "InputClass"
   	Identifier      "system-keyboard"
   	MatchIsKeyboard "on"
   	Option          "XkbLayout" "fr"
   	Option          "XkbVariant" "oss"
EndSection
```
20-intel.conf:
```xf86conf
Section "Device"
   	Identifier  "Intel Graphics"
   	Driver      "intel"
   	Option      "DRI" "2" # DRI3 is now default
   	#Option      "AccelMethod" "sna" # default
   	#Option      "AccelMethod" "uxa" # fallback
    Option      "TearFree" "true"
EndSection
```
• Installer mes dotfiles (problème avec le start de slim et NetworkManager si fait après le reboot)
```sh
# Si fait avant le reboot, modifier le fichier /etc/sudoers en rajoutant NOPASSWD: temporairement

git clone git://github.com/Quent42340/dotfiles.git
mv dotfiles .dotfiles
bash ~/.dotfiles/bin/dotfiles
# configurer fish
timedatectl set-ntp true
```
• Redémarrer, puis relancer `~/.dotfiles/init/11_systemd.sh`
