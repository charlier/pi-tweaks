Wipe out everything we don't want (from Raspbian Jessie Lite).
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
sudo apt-get install omxplayer
```

Update any firmwares etc.
```bash
rpi-update
```

Disable bootspam (& overclock the NIC) in /boot/cmdline.txt by appending
```
logo.nologo quiet smsc95xx.turbo_mode=y
```

Overclocking options appended to /boot/config.txt
```
#dtparam=audio=on		#Comment out to disable audio

#CPU
arm_freq=1000                   #Frequency of ARM processor core in MHz (default 700)
over_voltage=6                  #ARM/GPU voltage adjust, values over 6 voids warranty (default 0)
core_freq=500                   #Clock frequency for the VPU (default 250)

#RAM
gpu_mem=256                     #Give the GPU a load of memory (default 64)
sdram_freq=600                  #Frequency of SDRAM in MHz (default 450)
sdram_schmoo=0x02000020         #Set SDRAM schmoo to get more than 500MHz freq (default unset)
over_voltage_sdram_p=6          #SDRAM phy voltage adjust (default 0)
over_voltage_sdram_i=4          #SDRAM I/O voltage adjust (default 0)
over_voltage_sdram_c=4          #SDRAM controller voltage adjust (default 0)

#SD Card
dtparam=sd_overclock=100        #Clock in MHz to use for MMC microSD (default 50)

# Limits
boot_delay=1			#Helps to avoid sdcard corruption (default 0)
disable_splash=1		#Avoids the rainbow splash screen on boot (default 0)
```

Disable root login, password login etc. in /etc/ssh/sshd_config
```bash
PermitRootLogin no
PasswordAuthentication no
X11Forwarding no
```
Pray you've added the correct authorized_keys

Play a 1080p video:
```bash
omxplayer --live rtmp://<server_ip>/live/foo
```

## On a remote PC

Assuming you've already got Docker installed, fire up a RTMP server:
```bash
docker run -d -p 1935:1935 --name nginx-rtmp tiangolo/nginx-rtmp
```

Run ffmpeg to output to rtmp
```bash
ffmpeg -re -i big_buck_bunny_1080p_h264.mov -c copy  -f flv rtmp://<server>/live/foo
```
