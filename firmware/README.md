## AAMP software operation
The software operation at a high level is defined at a collaboration of two devices: IoT battery pack and RPi with their roles defined as follows:

 * Battery pack wakes up reguralry from deepSleep and doesthe following:
   * Checks all measurements, evaluates what power is available
   * Decides if RPi needs to be waken up
   * Tries to connect to management network and report
   * Tries to connect to RPi network and report
   * Creates an AP if that fails
 * Battery pack wakes up RPi on a given schedule or external conditions
   * Battery voltage above 90%
   * PIR sensor detected something
   * Scheduled heartbeat boot
 * When RPi boots, the following happens
   * RPi takes control of its own power, so battery pack can not turn it off, turns of itself as configured
   * RPi creates and AP, IoT battery pack joins it
   * RPi fetches measurements from battery pack via network: `<IP of battery pack>/json`
   * RPi can configure and re-configure rules on battery pack
   * IoT battery pack goes back to sleep after 120s when RPi booting started
   * RPi can enable 5GHz router for network connectivity


## IoT battery pack EasyESP configuration and rules

The entire power handling process is defined in rules as implemented in EasyESP, for example:

```
On System#Boot do //on boot
   timerSet,1,30      // set timer1 in 30s
 endon

On Rules#Timer=1 do  //on timer1
   timerSet,2,120       // set timer2 in 60s
   gpio,15,1 //turn on rpi
endon

On Rules#Timer=2 do  //on timer2
  //deepSleep,60 //sleep for 120s
endon

On System#Sleep do //on sleep
  gpio,15,0 //turn all off
  gpio,12,0
  gpio,14,0
endon

```

 
## RPi Zero W config

 * Enable serial: http://spellfoundry.com/2016/05/29/configuring-gpio-serial-port-raspbian-jessie-including-pi-3/
 * Enable ssh: add ssh.txt to sd card
 * Boot device and change credentials
 * Configure GPIO pins in `/etc/rc.local`
   * GPIO 17 - RPi self power, battery pack enables it and wait for RPi to boot, then while this pin is high, power to RPi can not be turned off, self shutdown is possible then.
   * GPIO 27 - Router power, while GPIO is high the 5GHz router is powered on.
  
 ```
echo "17" > /sys/class/gpio/export
echo "out" > /sys/class/gpio/gpio17/direction
echo "27" > /sys/class/gpio/export
echo "out" > /sys/class/gpio/gpio27/direction
echo "1" > /sys/class/gpio/gpio17/value
echo "1" > /sys/class/gpio/gpio27/value
```

## RPi Zero W pinout
The following pinout is used on the devices:

1. - 3.3V - orange -RTC power
1. - 5V - red - PIR power
3. - SDA - yellow - RTC SDA
4. - 5V - white - power in
4. - SCL - green - RTC SCL
6. - GND - black - power in
7. - GPIO4 - brown - PIR signal
8. - GPIO14 - UART TX - NC
9. - GND - blue - RTC power
10. - GPIO15 - UART RX - NC
11. - GPIO17 - purple connected to battery pack, enable self
12. - GPIO18 - random color - connected to battery pack, enable 5GHz router
13. - GPIO27 - gray - connected to battery pack, enable mosfet output
14. - GND - random color - connected to PIR

## Light sensor cable pinout
Flat IDC6 cable connector on one side, soldered to TSL light sensor on other side.
 * Pin 1 - red wire - SCL
 * Pin 2 - gray - SDA
 * Pin 5 - gray - VCC
 * Pin 6 - gray - GND
 
 ### Function test with Raspbian
 Testing all the functions of the system is possible with the following setup:
  * Install Raspbian Lite https://www.raspberrypi.org/downloads/raspbian/
  * Add file named `ssh` to boot partition to enable headless connectivit
  * Run `sudo raspi-config` and enable i2c, camera and expand the filesystem 
  * Install i2c-tools https://learn.adafruit.com/adafruit-16-channel-servo-driver-with-raspberry-pi/configuring-your-pi-for-i2c
  * Test PIR sensor, note pin numbers have to be changed: https://diyhacking.com/raspberry-pi-gpio-control/
  * Test self-enable power on RPi
  
 
