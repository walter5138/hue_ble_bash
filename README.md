# hue_ble_bash
Project controlling Philips Hue Bluetooth color and white bulbs. Bash version.

Project controlling Philips Hue Bluetooth color and white bulbs using:
- Bluez
- Dbus
- Bash
- Philips Hue Bluetooth color and white bulbs ( LCA001 tested),
  and with all other Philips Hue lamps with the same characteristics.


It is a learning project and there are many improvementes to make,
But you can:
- connect the bulbs
- switch the bulbs on and off
- set the ac_on_state
- set, increase/decrease brightness
- set, increase/decrease mired ( color tempereature )
- set the color for a single bulb or all
- have different color loops
- change transition time
- alarm funktion: ping or blink  
- set the bulb name

It demonstrates what the characteristics do.
Aware of that you can use them in your own projects.

This project is in daily development.
Therefore it is not granted to work for everybody.

If you are just interested in what the characteristics do
watch the file hue_debug.


Following prerequisites are required:

1. A running Linux system.

2. Build Dbus with the send-variant-dict.patch.

   https://chromium-review.googlesource.com/changes/chromiumos%2Foverlays%2Fchromiumos-overlay~12323/revisions/2/files/sys-apps%2Fdbus%2Ffiles%2Fdbus-1.4.12-send-variant-dict.patch/download

   Please read: https://stackoverflow.com/questions/8846671/how-to-use-a-variant-dictionary-asv-in-dbus-send/38573254 .
   This patch is for the dbus-send utility to get the capability to send the data type dict:string:variant a{sv} .
   Dbus understands a{sv} but dbus-send can't send it out of the box, therefore the patch.

3. Dbus running with system bus

4. The Linux bluetooth-stack Bluez build with Dbus

5. 3 Philips Hue Bluetooth color and white light bulbs ( LCA001 )
   If you have more or less than 3 Bulbs you have to alter some scripts
   at the moment. See TODO.


Get it working:

Once the prerequisites are in place you need to:

Use bluetoothctl to pair and connect the bulbs:
- go to a terminal
- at the prompt input bluetoothctl
- check if the Bluetoothcontroller is available: list
- power on the bluetoothcontroller: power on 
  If you want the bluetoothcontroller to be powered on as soon it's available (while boot)
  go to /etc/bluetooth/main.conf and uncomment "AutoEnable=true" at the bottom of the file
- make the controller pairable: pairable on
- scan for new devices: scan on
- pair the bulb with the address: pair XX:XX:XX:XX:XX:XX
- trust the bulb: trust XX:XX:XX:XX:XX:XX
- connect the bulb: connect XX:XX:XX:XX:XX:XX

Set the Bulb addresses in hue_settings:
- Insert the Bulb(s) address(es) eq. XX:XX:XX:XX:XX:XX in the file hue_settings.
  Convert XX:XX:XX:XX:XX:XX to XX_XX_XX_XX_XX_XX for dbus to understand it.
- If you have another number of bulbs see TODO.

Have fun controlling the bulbs: ./hue_...



Debug

Use -d or --debug parameter to the scripts to see values of all known characteristics
when the script finishes.


Tuning

Append to /var/lib/bluetooth/"adapter_id"/"hue_bulb_id"/info :
 
[ConnectionParameters]

MinInterval=6

MaxInterval=7

Latency=0

Timeout=216
	
Reduces the response time for the dbus-send commands.


Todo

- Commend the scripts!
- Work on README.
- Dig into these ConnectionParameters in /var/lib/bluetooth/"adapter_id"/"hue_bulb_id"/info to set values that make
  more sense !!!
- Figure out the bulbs color transition !!!
  In short: the color is representet with 4 hex values eq. "37 27 2c 0c" for blue. This values could be CIE color gamut xy
  coordinates or hue and saturation.
  I send a kind of RGB values which are translatet IN THE BULB! eq. "0x01 0xfe 0x01 0x01" is translated to "05 b1 ec 4e" for red.
  If you are interested in further detail i can describe much more.
- Alter the scripts to use different number of Bulbs.
- Hue Bulbs are just able to pair once. If you attempt to pair one more time eq. you want to pair with another controller,
  you have to do a factory reset! To do so you need a Philips remote. Hopefully there is another way doing it.

Questions:

- How to send color values the right way? What are the sent back values from the bulbs representing? What data structure
  is behind it?

Objectives:

- Learn about Bluetooth Low Energy.
- Get familiar with Linux bluetooth stack Bluez.
- Make those GATT characteristics available.
- Beginning with bash, porting it to python, covert it to c, becomming a BLE-Developer (;-).  
- It should be a learning project which clarifies BLE and Bluez using Linux making it more usable for all who are interested in.

