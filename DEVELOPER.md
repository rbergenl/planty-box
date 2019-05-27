## Raspberry Initial Setup
Raspberry install: SSID <NOOBS; have usb mouse, keyboard and hdmi display/projector.

### Install virtual keyboard
- https://raspberrypi.stackexchange.com/questions/41150/virtual-keyboard-activation.

## Enable Networking
Have Wifi country code set (via voorkeuren).
Then `$ ifconfig` to get wlan0 ip.
Then `$ sudo raspi-config` interfacing options > enable SSH. (and modify the password)
Make sure SSID without special characters (iphone: settings>general>about)
generate encoded password with `$ wpa_passphrase "testing"`
set (sudo nano) ./etc/wpa_supplicant/wpa_supplicant.conf with
```
network={
  ssid="iPhone"
  psk="pass"
}
network={
        ssid="VGV7519C32FE6"
        psk="Amtg67QfXEEa"
}

```
Then from laptop "ssh pi@192.168.2.9" (default pw: raspberry (updated to: robin))

## Install NodeJS
- `$ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -`
- `$ sudo apt-get install -y nodejs`

## Bluetooth Advertise as Peripheral
- Flow: on startup listen to button press; then when pressed enable bluetooth for a period; then when bluetooth is enabled advertise as peripheral; then when text is received set that as the network settings in wpa_supplicant and restart the wifi
- `$ sudo apt-get install bluetooth bluez libbluetooth-dev libudev-dev`
- `$ mkdir bluetooth && cd bluetooth && npm init -y`
- `$ npm install bleno`
- Create a file `start.js`:
```javascript

```
- Create a file `setWifi.js`:
```javascript
exec('echo "password" > temp_pass');
exec('wpa_passphrase "ssid" < temp_pass | sed \'/#psk/d\' > temp_conf');
exec('printf \'%b\' "$(cat temp_conf)" >> /etc/wpa_supplicant/wpa_supplicant.conf');
exec('rm temp_pass && rm temp_conf');
exec('wpa_cli -i wlan0 reconfigure');
```
- Add startup command to `$ sudo nano /etc/rc.local`

## Temperature/Humidity sensor
- On laptop download example file fom pigpio website and run `scp DHT22.py pi@192.168.2.9:~/DHT22.py`
- SSH into the Pi
- sudo apt-get build-essential python-dev
- Run idle `/usr/bin/gksu -u root idle`
- Run `$ wget abyz.co.uk/rpi/pigpio/pigpio.zip`
- Run `$ unzip pigpio.zip`
- Run `$ cd pigpio` and `$ make` and `$ sudo make install`
- Run `$ sudo pigpiod` (to start the deamon) (to kill it run `ps -A` and then `sudo kill 1332`)

## Raspberry Ready for Shipping
- have bluethooth disabled by default; and have physical button to enable bluetooth for 1 minute
- have ssh disabled

## Electrical Circuit basics
- Know from the sensor the voltage-down value and the desired current (mA)
- Determine the input is 3.3v or 5.0v
- Use the formula to determine the needed resistance (ohm) value: V = I * R (voltage is the current times resistance).
- Use an online calculator to check the resistance color codes. If you see 2 blacks, hold that to the left; if you see a gold; hold that to the right. With 5 bands is "value1, value2, value3, multiplier, tolerance".
- Connect LED cathode (short leg) to ground (place resistance between), and the anode (long leg) to a GPIO pin.
- Connect Button lefttop leg to GPIO pin; and righttop leg to ground via resistance. Then, input 5v to rightbottom leg.

# Used Resources

## Hydroponics
- https://github.com/novemberalpha/openag_brain_pfc1_config

## Raspberry Pi
- https://www.w3schools.com/nodejs/nodejs_raspberrypi.asp
- https://www.raspberrypi.org/resources/learn/
- https://projects.raspberrypi.org/en/projects/physical-computing
- https://trinket.io/sense-hat
- https://www.dexterindustries.com/howto/run-a-program-on-your-raspberry-pi-at-startup/

## Electrical circuit basics
- https://www.electronics-tutorials.ws/logic/pull-up-resistor.html

## Bluetooth
- https://github.com/noble/bleno

## Temperature/Humidity sensor
- http://www.circuitbasics.com/how-to-set-up-the-dht11-humidity-sensor-on-the-raspberry-pi/

## AWS IoT
- https://docs.aws.amazon.com/iot/latest/developerguide/iot-sdk-setup.html
- https://github.com/aws/aws-iot-device-sdk-js
- https://medium.com/@rohanmaheshwari/using-aws-iot-with-the-js-sdk-node-to-turn-an-led-on-and-off-with-a-raspberry-pi-be43346a5bd4
- https://docs.aws.amazon.com/iot/latest/developerguide/protocols.html
- https://github.com/aws-samples/aws-iot-examples
- https://github.com/aws-samples/aws-iot-chat-example
