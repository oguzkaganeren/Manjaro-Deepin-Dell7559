# Manjaro-Deepin-Dell7559

manjaro-deepin-18.0.2-stable-x86_64 or new version.


```
sudo nano /etc/default/grub 
```
> GRUB_CMDLINE_LINUX_DEFAULT="quiet acpi_osi=! acpi_osi=\"Windows 2009\" acpi_backlight=vendor i915.modeset=1 scsi_mod.use_blk_mq=1" 
```
update-grub
```
### Fastest Mirror List
```
sudo pacman-mirrors --fasttrack
```
### Packages I use
```
sudo pacman -S --noconfirm --needed pavucontrol aria2 ttf-ubuntu-font-family rxvt-unicode unace unrar sharutils uudeview arj cabextract speedtest-cli virt-manager qemu vde2 ebtables dnsmasq bridge-utils openbsd-netcat tlp-rdw smartmontools ethtool x86_energy_perf_policy yay xf86-video-fbdev telegram-desktop kdenlive inkscape create_ap virtualbox
```
### Power Settings
```
sudo timedatectl set-ntp true
sudo systemctl enable libvirtd.service
sudo systemctl start libvirtd.service
sudo systemctl mask systemd-rfkill.socket systemd-rfkill.service
sudo sensors-detect
sudo systemctl enable thermald
sudo systemctl start thermald
```
About: https://forum.manjaro.org/t/howto-power-savings-setup-20180906/1445
### INTEL - Enable Early Kernel Mode Setting for i915 module.
Edit /etc/mkinitcpio.conf file and in MODULES section add i915.
```
# MODULES
# The following modules are loaded before any boot hooks are
# run.  Advanced users may wish to specify all system modules
# in this array.  For instance:
#     MODULES=(piix ide_disk reiserfs)
MODULES=(i915)
```
Save and
```
sudo mkinitcpio -P
```
### Colorful Yay
```
sudo gedit /etc/pacman.conf
```
Change `#Color` to `Color` below the Music options.

### Aur Packages I use
```
yay -S --noconfirm materia-theme opera chromium spotify ttf-font-awesome ttf-font-awesome-4 powerline-fonts ttf-roboto adobe-source-sans-pro-fonts android-studio woeusb-git visual-studio-code-bin papirus-icon-theme ntfs-3g jdownloader2 ttf-ms-fonts ephifonts otf-exo thermald
```
### For Other Partitations
If you have another partition(E, D etc.). You can mount it on the startup. Thus some applications which are using other partitions don't get an error.

```
lsblk -f
```
The command shows your disks uuid.
```
sudo gedit /etc/fstab 
```
Open your fstab config with the command. You should add codes similar to the following example. You should change UUID and /run/media/yourUserName/Partition.
```
UUID=DAF6FE7CF6FE5869 /run/media/oguz/D ntfs-3g defaults  0 0
UUID=C480917680917022 /run/media/oguz/E ntfs-3g defaults  0 0
```

>  :exclamation: If you use manjaro with dual boot, you should close fast-startup,hibarnate on your Windows, otherwise, you have not a write permission for other partitions.
>  :exclamation: When your headphone connects your computer then if you restart your computer or you connect it before startup, alsa not select your headphone and you should re-plug or you can fix it with;
```
sudo gedit /etc/pulse/client.conf 
autospawn = yes
```
### Installing Nvidia Drivers
Open System Settings or Manjaro Settings>Drivers, then click Auto Install Proprietary Drivers.


After Installing restart your computer. Done.
### Firefox screen tearing during scrolling Issue
```
sudo gedit /etc/profile.d/kwin.sh
```
then put this in it,
```
#!/bin/sh

export KWIN_TRIPLE_BUFFER=1
```
After that, 
```
sudo gedit ~/.config/kwinrc
```
Add those parameters at the bottom,

```
MaxFPS=60
RefreshRate=60
```
Save it and reboot. 
Open firefox and 
**about:config**
then search  **layers.acceleration.force-enabled**
It should be true.
Done.
### If you want to edit your host file
```
sudo gedit /etc/hosts
```
>  :exclamation: If you have a SSD, you should enable fstrim.
```
sudo systemctl enable fstrim.timer
```
### Open Wifi Hotspot
```
sudo create_ap wlp5s0 wlp5s0 MyAccessPoint password
```
### Terminal PS1
```
sudo gedit .bashrc
```
Change PS1 with this;
```
PS1='\[\033[0;32m\]\[\033[0m\033[0;32m\]\u\[\033[0;36m\] @ \[\033[0;36m\]\h \[\033[0;32m\]$(git_branch)\n\[\033[0;32m\]└─\t\[\033[0m\033[0;32m\] \$\[\033[0m\033[0;32m\] ▶\[\033[0m\] '
```
and add this lines at the bottom;
```
git_branch() {
    git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
```
![Image](https://user-images.githubusercontent.com/5963437/46868048-baf1e580-ce2f-11e8-97aa-a02be1b8a066.png)


### For Backup
```
sudo pacman -S timeshift
```
### DVD/CD Mounting
If you have a external CD/DVD,
```
sudo pacman -S k3b
```
### PostgreSQL(for me)
```
sudo pacman -S postgresql
sudo su postgres -l # or sudo -u postgres -i
initdb --locale $LANG -E UTF8 -D '/var/lib/postgres/data/'
exit
sudo systemctl enable --now postgresql.service
sudo pacman -S pgadmin4

```
## Some Errors and fixer
### Virtualbox
#### NS_ERROR_FAILURE (0x80004005) Error
```
Result Code: 
NS_ERROR_FAILURE (0x80004005)
Component: 
MachineWrap
Interface: 
IMachine {85cd948e-a71f-4289-281e-0ca7ad48cd89}
```
This is the error and we can fix it with this;
```
sudo pacman -Syyu
sudo pacman -S virtualbox-host-dkms
sudo pacman -S linux-headers
sudo modprobe vboxdrv
sudo /sbin/rcvboxdrv setup
```
#### VirtualBox: Error -610 in supR3HardenedMainInitRuntime!
This is the error.
```
VirtualBox: Error -610 in supR3HardenedMainInitRuntime!
VirtualBox: dlopen("/usr/lib/virtualbox/VBoxRT.so",) failed: <NULL>

VirtualBox: Tip! It may help to reinstall VirtualBox.
```
Maybe it can fix with(I didn't try it);
```
sudo chmod -002 /usr/lib/virtualbox
```
Or, I fix it with;
```
sudo chmod -002 /usr
```
