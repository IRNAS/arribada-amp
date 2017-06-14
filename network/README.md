## Network configuration for AAMP
The network is configured mostly with static IP addresses to speed-up the boot time of the network and avoid potential problems.

### Devices in the network
 * Main router: 192.168.1.1/24 with DHCP range .100-200
 * Base station 5GHz: 192.168.1.30
 * Base station 2.4GHZ: 192.168.1.31
 * Camera units:
   * Unit 0: 192.168.1.50
   * Unit 1: 192.168.1.51
   * Unit X: 192.168.1.(50+X)
