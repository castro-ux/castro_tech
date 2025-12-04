
Part II installation

### Set local time
- ln -sf /usr/share/zoneinfo/US/Eastern /etc/localtime
- hwclock --systohc
- date

### locale-gen and editor
- locale-gen 
    //need vim to edit files though
- pacman -Syu vim which sudo man-db man-pages texinfo intel-ucode lvm2 iwd openssh bash-completion
- pstree

## Networking- to autostart services at startup
- systemctl enable systemd-networkd.service
- systemctl status systemd-networkd.service

systemctl enable systemd-resolved.service

- /etc/systemd/network/25-wireless.network
- vim !!
    [Match]
    Name=wlan0

    [Network]
    DHCP=yes
    IgnoreCarrierLoss=3s

- cat /etc/systemd/network/25-wireless.network
- systemctl enable iwd.service

## Mkinitcpio
- vim /etc/locale.conf
    LANG=en_US_UTF-8
     —or—
    echo “LANG=en_US_UTF-8” >> /etc/locale.conf
    
- vim /etc/locale.gen
    //uncomment en_US.UTF-8 UTF-8
- locale-gen
- cat /etc/locale.conf
- vim /etc/vconsole.conf
     KEYMAP=us
- vim /etc/hostname 
    cloe

### Configuring mkinitcpio
- *man mkinitcpio* for documentation
- vim /etc/mkinitcpio.conf
    HOOKS=(base systemd autodetect microcode modconf kms keyboard 
    sd-vconsole block sd-encrypt lvm2 filesystems fsck)
     // line right above COMPRESSION
- mkinitcpio -P

## Bootloader
- lsblk
- bootctl install
- bootctl update

### Loader Configuration
- ls /boot
-        //confirm entries, EFI folder in there
- vim boot/loader/loader.conf
      timeout 4
      default arch.conf
      console-mode max
      editor no

- vim boot/loader/entries/arch.conf
    title Arch Linux
    linux /vmlinuz-linux
    initrd /initramfs-linux.img

    options rd.luks.name=UUID=archie       root=/dev/archie/root  rw


### UUID
- blkid
- vim arch.conf
- :.!blkid //visual mode sda2

## Add username 
- useradd -m -G wheel cloe
- groups cloe
- visudo
    //uncomment wheel %wheel ALL=(ALL:ALL) ALL
- passwd cloe 

### Test
lsblk
ls /dev/mapper
   Should have:
   /dev/mapper/archie-root
   /dev/mapper/archie-swap
   
...
## Unmount and Reboot. Back in root
- “exit” to leave the change root shell. 'root' will now be red root@archiso
- umount -R /mnt
- swapoff -a
-       Various target is busy warnings which is fine
- reboot
- Remove usb drive before startup

## Cross your fingers now!
you will be prompted with yur login in ttyl mode
Arch Linux 6.17.8-arch1-1
cloe login:
Password


