# ArchInstall-Guide
a Guide for arch linux

arch linux install guide (UEFI ONLY)

step one - make a UP TO DATE ARCH USB (if you dont know how to make one arch isnt for you prob)
(USE THE ARCH LINUX WIKI- https://wiki.archlinux.org/title/Installation_guide)

now if your lazy you can use the `arch-install` cmd. it makes things easier

ALSO A TIP "ctrl L" clears the screen, `lsblk` shows partitions and drives

package manager being a Female DOg???? - `pacman -Sy archlinux-keyring`
----------------------------------------------------------

PREINSTALL
1. check your keboard layout (by default arch uses the US keyboard layout)

if you want to change your keyboard layout you can always use for example `loadkeys it`. that will change it to the pasta keyboard layout

2. check your internet connection

(for the love of god use ethernet its soooo much easier lol tho if not use this - https://wiki.archlinux.org/title/iwd#iwctl)

to check if you have internet simply type in `ping chuckecheese.com` if it say like 64 bits from it you have internet (LETS GO). hit ctrl C to clear it
-----------------------------------------------------------

PARTITONING DRIVES
--note-- dont use fdisk that shit hard. use cfdisk instead. also this is just a general of what youll need.

1. type in `cfdisk`

since you are using `cfdisk` and using `uefi` your gonna want to use gpt you dingle berry

--another note-- if it doesnt show up the right drive type in `cfdisk /dev/(whatever the drive is lol)`

2. Create a 100M partition(your EFI partition), create a 4G partition (swap), and use the rest for your last partition (your root)

--more notes-- you may make your swap partition any size you want, it is your "oh shit my rams full partition" without it if your ram is full your computer will just crash (not fun unless your windows 98).

3. write it before you forget (you dont need to fiddle with the types, we are gonna formate the drives later)

4. quit and ctrl L

-------------------------------------------------------------

FORMAT DRIVES
1. open up `lsblk` to keep a heads up on your partitions

2. format your root partition - `mkfs.ext4 /dev/(root part)`

its gonna show up with shit thats normal

3. format your EFI partition - `mkfs.fat -F 32 /dev/(EFI part)`

4. "format" your swap partition - `mkswap /dev/(your swap part)`

------------------------------------------------------------------
MOUNTING partitions
--how to setup BRTFS if you perfer that lmao
https://www.arcolinuxd.com/installing-arch-linux-with-a-btrfs-filesystem/


--note-- always mount root first

1. mount your root partition - `mount /dev/(root part) /mnt`

2. make your boot directory - `mkdir -p /mnt/boot/efi`

3. mount your boot partition - `mount /dev/(boot part) /mnt/boot/efi`

4. turn ya swap on - `swapon /dev/(SWAP part)`

------------------------------------------------------------------
INSTALLING THE BASE SYSTEM

1. install your shit - `pacstrap /mnt base linux linux-firmware sof-firmware base-devel grub efibootmgr nano networkmanager` dont forget to install the ucode for your cpu!!!!

--note-- base linux and linux firmware is only what is really needed for boot, nano is our text editor there are many options but nano is easy, base-devel is for AUR, grub is the bootloader,

after it installs its time for the next step

-------------------------------------------------------------
CONFIGURE THE SYSTEM

--note `genfstab /mnt` shows your partitions and what they are booted too--

1. write that fstab file you just saw in the note and put it in your system - `genfstab -U /mnt > /mnt/etc/fstab`

--check it using `cat /mnt/etc/fstab`

TIME TO ENTER INTO ROOT LETS GOOOOOOO

2. enter into the root of your system - `arch-chroot /mnt`

--since i live near the central time zone i will use that time zone here, i am too lazy to tell you how to find yours sorry--

3. set your time zone -- `ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime`

4. set your hardware clock - `hwclock --systohc`

5. configure more local stuff using nano, (remove the hashtag for the area you want) - `nano /etc/locale.gen` then ctrl O, enter, then ctrl x

6. use the `locale-gen` cmd to add it to your system

7. add `LANG=en_US.UTF-8` to /etc/locale.conf using nano

8. make your hostname - `nano /etc/hostname` and type a easy to remeber name for your computer

9. write a root password - `passwd` then just type a password berrys and cream

10. create a user - `useradd -m -G wheel -s /bin/bash (a username idk)` (-m means to make and -G means to add to a group)

11. create a password for that user - `passwd (user)`

--------------------------------------------------------
setting up sudo

12. use `EDITOR=nano visudo` and uncomment "%wheel ALL=(ALL) ALL"
(anyone apart of the wheel group can use sudo cmds now yay)

--enable networkmanager--

13. use `systemctl enable NetworkManager`

--setup your bootloader--

14. use `grub-install /dev/sda`

15. `grub-mkconfig -o /boot/grub/grub.cfg`

--note this doesnt include os-prober, that is needed for grub to find other partitions to boot to--

exit then exit again then, type `reboot` and pray this all worked

------------------------------------------
Setup KDE and other things (not needed but useful)

1.log in with your user

2.check your internet with the ping cmd

--if tiny screen `setfont -d` will make it bigger--

3. `sudo pacman -S plasma sddm`

4.just press enter on all if you want lol

5. `sudo pacman -S konsole kate firefox`

6.`sudo systemctl enable sddm` (this only alow at startup good but me want it now)

7. `sudo systemctl enable --now sddm`
------------------------------------------
Gnome is very simular just without installing SDDM and using `systemctl enable gde` instead
__________________________________________
now go pretty up your arch install bois have fun

DONT FORGET TO INSTALL NEOFETCH FOR YOUR FLEXING NEeds
just type `neofetch` into the terminal and it shall show up

CREID TO THIS VIDEO THIS GUY IS A POGCHAMP - https://www.youtube.com/watch?v=68z11VAYMS8&list=LL&index=4
