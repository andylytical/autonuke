# autonuke
Script to shred all disks found on a host

# Simple usage on a single host
Assume logged in as root (usually default for SystemRescueCD)

1. git clone
1. ./autonuke

# Wipe multiple machines all on same network
Nodes must all be connected to a common network.
One node will act as master and serve DHCP and PXE boot requests.
## Boot one host as master host
See also: http://www.sysresccd.org/Sysresccd-manual-en_PXE_network_booting

1. Boot systemrescuecd from cd
1. Configure network settings:
   * /etc/cond.f/network
   ```
   ifconfig_eth0=”192.168.0.5 netmask 255.255.255.0”
   defaultroute=”gw 192.168.0.254”
   ```
1. If more than 50 hosts:
   1. Change PXEBOOTSRV_DHCPRANGE in /etc/conf.d/pxebootsrv
1. /etc/init.d/pxebootsrv start

## Boot additional hosts to have their hard drives 'shred'ed

1. Edit /tftpboot/pxelinux.cfg/default
   ```
   append …  ar_source=http://192.168.1.5 setkmap=us
   ```
1. Copy contents of `autonuke` to `/tftpboot/autorun`
1. Any node that PXEboots on this network will automatically shred all hard drives without prompting.
