# esphome_huawei_sun2000
Read your Huawei Sun 2000 inverter data from Home assistant using Modbus RTU from an ESP32

![imagen](https://user-images.githubusercontent.com/18996263/174140692-747c9744-731f-406d-bb1b-857c47be10b2.png)

Hardware used:
* ESP32 DEVKIT V1. Any ESPhome capable board should work, but I have only tested with this board. If using a ESP8266, read the warning on the [Modbus controller ESPhome documentation](https://esphome.io/components/modbus_controller.html)
* RS485 to TTL module
* Huawei SUN2000 solar inverter with the smart energy monitor

# Wiring
* Connect The ESP RX pin to the RS485 module's RX
* Connect The ESP TX pin to the RS485 module's TX
* Connect the RS485 module's A pin to pin the free modbus A pin on your inverter's COM port (pin 1)
* Connect the RS485 module's B pin to pin the free modbus B pin on your inverter's COM port (pin 2)
* If using a shielded cable, highly recommended, connect the shield to GND on ONLY ONE SIDE

Check the manual of your inverter to locate the com port. The pin numbers are visible in the port itself.

# Setup
1. Adapt the [esphome_huawei.yaml](esphome_huawei.yaml) to your liking and flash it using your method of choice
2. Connect wires as indicated in the Wiring section
3. Connect the ESPhome device to Home Assistant as usual
4. Add the [home_assistant_huawei.yaml](home_assistant_huawei.yaml) configuration to your Home Assistant configuration. I have it setup as a package, but you might need to split the configuration according to your setup
5. Add the new entitites to lovelace. In [ui-lovelace.yaml](ui-lovelace.yaml) you will find a basic example. Tweak to your liking

# Tweaking read timings
There are two timings that can be setup in the ESPhome modbus_controller configuration:
* update_interval controls how often the inverter is polled for new data. Set it to a low number if you want to see smooth curves and also have almost instant power readings. If you just want to use the Energy pannel, you can set it to a much lower frequency and reduce the load on your system
* command_throttle controls n each polling cycle how much time to wait between reading non consecutive registers. During each update_interval, several register ranges are polled and a command_throttle pause is waited in between. Setting this to 500ms or lower creates reading errors that ruin your energy statistics. I did not try anithing between 500ms and 750ms. Shorter time produces more accurate house power and current calculations

It should be possible to use the skip_updates feature to produce less energy readings while keeping a smooth update of power readings. If you try and suceed, feel free to create a PR.

# The usual disclaimer
This setup did not break anything in my installation. However, I take no responsability for what happens in your installation. Use at your own risk.
Take care when dealing with the inverter and mains voltage. Disconnect the inverter from all power sources as indicated in the manual. I also take no responsabilities if you get hurt installing this.

To make it clear... I take no responsability to whatever happens if you use this instructions or the code contained.
