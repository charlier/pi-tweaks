Remove packages we don't need (from Raspbian Jessie Lite)
```bash
sudo apt-get -y update
sudo apt-get -y dist-upgrade
sudo apt-get -y purge `sudo dpkg --get-selections | grep -v "deinstall" | grep x11 | sed s/install//`
sudo apt-get -y purge `sudo dpkg --get-selections | grep -v "deinstall" | grep python | sed s/install//`
sudo apt-get -y purge `sudo dpkg --get-selections | grep -v "deinstall" | grep sound | sed s/install//`
sudo apt-get -y purge `sudo dpkg --get-selections | grep -v "deinstall" | grep gnome | sed s/install//`
sudo apt-get -y purge `sudo dpkg --get-selections | grep -v "deinstall" | grep lxde | sed s/install//`
sudo apt-get -y purge `sudo dpkg --get-selections | grep -v "deinstall" | grep gtk | sed s/install//`
sudo apt-get -y purge `sudo dpkg --get-selections | grep -v "deinstall" | grep desktop | sed s/install//`
sudo apt-get -y purge `sudo dpkg --get-selections | grep -v "deinstall" | grep gstreamer | sed s/install//`
sudo apt-get -y purge `sudo dpkg --get-selections | grep -v "deinstall" | grep avahi | sed s/install//`
sudo apt-get -y purge `sudo dpkg --get-selections | grep -v "deinstall" | grep dbus | sed s/install//`
sudo apt-get -y purge `sudo dpkg --get-selections | grep -v "deinstall" | grep freetype | sed s/install//`
sudo apt-get -y purge raspberrypi-artwork penguinspuzzle
sudo apt-get -y autoremove
sudo apt-get clean
```

Add docker, give the user easy access (assume you've created a new user, not stuck with pi)
```bash
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker pi
```

Disable bootspam in /boot/cmdline.txt by appending
```bash
logo.nologo quiet
```

Overclocking options appended to /boot/config.txt
```bash
#dtparam=audio=on		#Comment out to disable audio

#CPU
arm_freq=1400                   #Frequency of ARM processor core in MHz (default 1200)
over_voltage=6                  #ARM/GPU voltage adjust, values over 6 voids warranty (default 0)

#RAM
total_mem=1024			#Expose extra 16M of RAM
gpu_mem=16                      #Reduce the amount of available RAM to the GPU (default 64)
sdram_freq=575                  #Frequency of SDRAM in MHz (default 450)
sdram_schmoo=0x02000020         #Set SDRAM schmoo to get more than 500MHz freq (default unset)
over_voltage_sdram_p=6          #SDRAM phy voltage adjust (default 0)
over_voltage_sdram_i=4          #SDRAM I/O voltage adjust (default 0)
over_voltage_sdram_c=4          #SDRAM controller voltage adjust (default 0)

#SD Card
dtparam=sd_overclock=100        #Clock in MHz to use for MMC microSD (default 50)

# Limits
boot_delay=1			#Helps to avoid sdcard corruption (default 0)
disable_splash=1		#Avoids the rainbow splash screen on boot (default 0)

dtoverlay=pi3-disable-wifi	#Disable Wifi
dtoverlay=pi3-disable-bt	#Disable Blutooth
```

Disable root login, password login etc. in /etc/ssh/sshd_config
```bash
PermitRootLogin no
PasswordAuthentication no
X11Forwarding no
```
Pray you've added the correct authorized_keys
