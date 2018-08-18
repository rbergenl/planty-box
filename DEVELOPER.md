Raspberry install: SSID <NOOBS; have usb mouse, keyboard and hdmi display/projector.
Have Wifi country code set (via voorkeuren).
SSID without special characters (settings>general>about)
set (sudo nano) ./etc/wpa_supplicant/wpa_supplicant.conf with
```
network={
  ssid="iPhone"
  psk="pass"
  key_mgmt=WPA-PSK
}
```
Install virtual keyboard: https://raspberrypi.stackexchange.com/questions/41150/virtual-keyboard-activation.

Then "ifconfig" to get wlan0 ip.
Then "sudo raspi-config" interfacing options > enable SSH.
Then from laptop "ssh pi@192.168.2.9" (default pw: raspberry)
