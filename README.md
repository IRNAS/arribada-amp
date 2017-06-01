# Arribada Arboreal Monitoring Platform
Camera based Arboreal Monitoring Platform with WiFi connactivity and solar power for remote area deployment. The solution is designed with open hardware and software solutions.

## Design overview
The network consists of one or more WiFi base-stations and a number of camera units deployed in the tree tops with a line of sight to the base-station.

### Base station
 * 5GHz sector or omni antenna, default choice Ubiquity Rocket AC PTMP + antenna
 * 2.4GHz sector or omni antenna, defautl choice Ubiquiti Nanostation M2
 * Backup battery
 * Battery charger
 * Satellite or other uplink
 
### Camera unit
 * Raspberry Pi Zero W
 * Raspberry Pi v2 Camera
 * Iot Battery Pack v2
  * solar charger
  * 3v3 regualtor module
  * 5v regulator module
  * 18V step-up module
  * 6x 18650 LiPo cells 2600mAh
 * Nodemcu v3 LP processor + EasyESP firmware
 * Voltaic system 17W solar panel
 * Nanuk 925 rugged enclosure
 * Tree moounting brackets
 
#### Software operation
Nodemcu v3 LP processor with ESP8266 is the main controller for the camera unit and turn on Raspberry Pi when required/scheduled.
 
#### EasyESP custom features
 * Disable AP-failover for soem scenarios/features
 * Enable operation without WiFi after wake-up from deep sleep, decide on doing WiFi given a set or conditions
 * Sequencing of devices after deep-sleep, first read values, then do logic or display operations.
 * Module: Wake up external device / turn on GPIO base on one of the following configurable conditions
  * Scheduled wake-up, for example every 30min with additional conditions, for example if some variable is greater or lower then X
  * Configuration via WiFi or UART
 * Module: Report/listen via LORA (optional and not very required)
 
 
