#Notes while installing linux on physical hardware first time

I goes with minmal installation didn't install any desktop envirnement.
Wanted to trying out terminal more so goes with windows manager dwm getting familiar with alot ins and out of operating system now.
Got familiar with systemd-boot and grub because i install linux mulitple times first nixos and then debian (4, 5 times :))

#Ways to Connect with wifi

##wpa_cli:
> add_network 
> set_network x ssid "myssid"
> set_network x psk "mypassword"
> set_network x scan_ssid 1
> enable_network x

##wp_supplicant:
wpa_passphrase myssid mypassword | sudo tee /etc/wpa_supplicant.conf
sudo wpa_supplicant -B -c /etc/wpa_supplicant.conf -i wlp2s0
sudo dhclient wlp2s0

if SSID is hidden then add this flag in wpa_supplicant.conf file generated in above step
add scan_ssid=1 in config file for hidden network

to auto connect to wifif on system start 
edit wap_supplicant.service file

#using Network Manager:

sudo apt install network-manager

nmcli radio wifi						--> check if wifi is enabled
nmcli radio wifi on 					--> to enable wifi
nmcli dev wifi list						--> previously connected or added wifi, available networks
sudo nmcli dev wifi connect "myssid" password "mypassword"
nmcli con show
nmcli con down "ssid/uuid"
nmcli con up "ssid/uuid"
///connect to hidden network
nmcli con add type wifi con-name <connect name> ifname wlp2s0 ssid <myssid>
nmcli con modify <connect name> wifi-sec.key-mgmt wpa-psk
nmcli con modify <connect name> wifi-sec.psk <mypassword>
nmcli con up <connect name>
nmcli dev
ip link show


# Mounting hard disk
lsblk or blkid to view disks and partitions
create dir to mount

mkdir /mount/path
mount /dev/sda1 /mount/path
umount /dev/sda1

remount a path with different flags/options
mount -o remount,rw /mount/path


#Software install
apt install sudo vim git curl build-essential
after installing sudo setup user as suoders by adding user to sudo group by adding user in suoders file directly
usermod -aG sudo myname
visudo

sudo apt install xorg stterm xterm suckless-tools libx11-dev libxinerama-dev libxft-dev
sudo apt install firefox-esr

##Buil DWM
generate config.h using make config.h target
update config.h 
set fonts, colors, layouts, shortcuts
sudo make clean install

setup some permission issue of XServer by adding following entries in  /etc/X11/Xwrapper.config
allow_users=anybody
need_root_rights=yes

create .xinitrc file in user's home directory 
  set xsetroot "SOME HEADER"
  xrandr --output eDP-1 --mode = 1920x1080 
  exe dwm

start Xserver using 
xinit

Exploration continue...
