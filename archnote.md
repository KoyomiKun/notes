## 安装arch

1. 制作启动盘

2. 在进入命令行之前输入`nomodeset vedio=800x450` 

3. 修改字体`setfont /usr/share/kbd/consolefonts/LatGrkCyr-12x22.psfu.gz` 

4. 修改键盘esc和大写锁定键`vim keys.conf` , 输入`keycode 1 = Caps_Lock` , `keycode 58 = Escape` .`loadkeys keys.conf` 

5. 互联网链接 `ip link` , `ip link set wlan0 up` , `iwlist wlan0 scan | grep ESSID` , `wpa_passphrase CW_WIFI passwd > internet.conf` , `wpa_supplicant -c internet.conf -i wlan0 &` , `dhcpcd &` , `ping baidu.com` 

6. 同步时间 `timedatectl set-ntp true` 

7. `fdisk -l` , `fdisk /dev/mmcblk1` ,  
