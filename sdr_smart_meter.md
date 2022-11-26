# sdr_smart_meter

# Hardware
Running in VM using Proxmox

OS:  Debian amd64 

SDR:  [Nooelec NESDR Smart v4 Bundle](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.amazon.com%2Fdp%2FB01GDN1T4S%3Fref%3Dppx_pop_mob_ap_share&data=05%7C01%7C%7Cbb7e1580191c463ef34f08dacfdc7be6%7C84df9e7fe9f640afb435aaaaaaaaaaaa%7C1%7C0%7C638050843119157377%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C3000%7C%7C%7C&sdata=2P3V8UsjjIiBnFdGSkw4I%2BwKkCB30Y0mUUby3V1T4WU%3D&reserved=0)

# Steps
Following the steps provided in [Smart Meter Dashboard: Collecting the Data](https://billyoverton.com/2016/06/19/smart-meter-collecting-the-data.html)

```
apt-get update
apt update
apt upgrade
apt-get install cmake build-essential libusb-1.0-0-dev git
apt-get install usbutils
apt install pkg-config
```
```
cd ~
git clone git://git.osmocom.org/rtl-sdr.git
cd rtl-sdr
mkdir build
cd build
cmake ../ -DINSTALL_UDEV_RULES=ON -DDETACH_KERNEL_DRIVER=ON
make
make install
ldconfig

nano /etc/systemd/system/rtl-tcp.service
```

```
[Unit]
Description=Software Defined Radio TCP Server
Wants=network.target
After=network.target

[Service]
ExecStart=/usr/local/bin/rtl_tcp
Restart=always

[Install]
WantedBy=multi-user.target
```
```
systemctl enable rtl-tcp.service
apt-get install golang
nano .profile
```
```
...
GOPATH=$HOME/go
PATH="$HOME/go/bin:$PATH"
...
```
```
go get github.com/bemasher/rtlamr
```
```
mkdir /opt/powermon
cp /home/pi/go/bin/rtlamr /opt/powermon
```

# References
- [Smart Meter Dashboard: Collecting the Data](https://billyoverton.com/2016/06/19/smart-meter-collecting-the-data.html)
- [bemasher's rtlmar Github](https://github.com/bemasher/rtlamr)
- [bemasher's rtlamr-collect Github](https://github.com/bemasher/rtlamr-collect)
- [BitBaningBytes gr-smart_meters Github](https://github.com/BitBangingBytes/gr-smart_meters)
- [USING SDR TO READ YOUR SMART METER](https://hackaday.com/2014/02/25/using-sdr-to-read-your-smart-meter/)
- [Gr Smart Meters Setup Guide](https://wiki.recessim.com/view/Gr-smart_meters_Setup_Guide)
- [USB Passthrough Proxmox](https://medium.com/@konpat/usb-passthrough-to-an-lxc-proxmox-15482674f11d)
