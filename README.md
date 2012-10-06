# Named Data Networking Link Protocol

NDNLP is a link protocol for delivering CCNx messages over a local one-hop link.

NDNLP provides the following two features:

* Fragmentation and Reassembly
* Acknowledgement and Retransmission

ndnld/ is an implementation of NDNLP.

Read [NDN Technical Report NDN-0006](http://www.named-data.net/techreport/TR006-LinkProtocol.pdf) for more details.

## SYSTEM REQUIREMENTS
### CCNx
ndnld has been tested with CCNx 0.6.0 and CCNx 0.6.2-rc1.

### Linux
ndnld has been tested with Ubuntu 11.10 and Ubuntu 12.04.

* [CUnit](http://cunit.sourceforge.net/) is required to run unit tests.

### FreeBSD
ndnld has been tested with FreeBSD 9.

* Unit tests do not compile on FreeBSD.
* UDP lower-layer with IPv6 addresses is always available.
* UDP lower-layer with IPv4 addresses is available when `ipv6_ipv4mapping="YES"` is specified in /etc/rc.conf.
* Ethernet lower-layer is not supported due to lack of AF\_PACKET.

### Mac OS
ndnld compiles on Mac OS X 10.6.8, but its functionality is not tested.

## USAGE
### Install
	cd ndnld/
	make
	sudo make install

On FreeBSD if you plan to use UDP lower-layer with IPv4 addresses, add `ipv6_ipv4mapping="YES"` to /etc/rc.conf, and reboot the machine.

### Start
	ccndstart
	ndnld

The program will daemonize itself.

### Stop
	killall ndnld

### Uninstall
	cd ndnld/
	sudo make uninstall

## CONFIGURATION
**ndnldc** is a command-line utility to configure ndnld.

ndnldc should be called *after* starting ndnld.
Configuration does not persist after ndnld is restarted.

### Create UDP Connection
Commands to create a UDP connection between r1 (192.0.2.1) and r2 (192.0.2.2):

	ndn@r1:~$ ndnldc -c -p udp -h 192.0.2.2
	ndn@r2:~$ ndnldc -c -p udp -h 192.0.2.1

*FaceID* will be echoed back.

### Create Ethernet Connection
Commands to create a Ethernet connection between r1 (eth1, 08:00:27:01:01:01) and r2 (eth2, 08:00:27:01:01:02):

	ndn@r1:~$ ndnldc -c -p ether -h 08:00:27:01:01:02 -i eth1
	ndn@r2:~$ ndnldc -c -p ether -h 08:00:27:01:01:01 -i eth2

*FaceID* will be echoed back.

### Register a Prefix
Commands to register a prefix on FaceID 11:

	ndn@r1:~$ ndnldc -r -f 11 -n ccnx:/example

### Other Commands
Please read section 3.4 of [technical report](http://www.named-data.net/techreport/TR006-LinkProtocol.pdf).


