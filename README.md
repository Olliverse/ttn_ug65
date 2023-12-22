# Gateway

The following describes the setup of the [ug65-l04eu-868m-ea](https://resource.milesight-iot.com/milesight/document/ug65-datasheet-en.pdf) and how it can be connected to the TTN.

![ug65.png](assets%2Fug65.png)

The antenna is designed for a frequency of 868MHz, although the gateway also supports other network areas. These include China's 470MHz and America's 915MHz, as well as others. All you have to do is use the right antenna. By default, the gateway comes with a web UI for configuration and should not be operated without an antenna.

# Change Eth Interface
To configure the Gateway so that it is able to connect to the Internet you have to use LAN and configure your eth interface so that it can reach the gateway.

## On Raspberry Linux

In order to configure the Lora gateway to send data to the TTN, it needs Internet access. By default, however, the gateway is set up as an access point, which means that it has no connection to the Internet. To change this, you have to connect to it via LAN and make adjustments through the web interface. Since the gateway is in a different network (192.168.*23.150*), the Ethernet interface must first be configured on the Raspberry. To do this, the following calls are made as root user "charon":

	`sudo nano /etc/network/interfaces.d/lora_eth.conf`

Then insert the following interface definition to be able to reach the gateway:

```
auto eth0
	iface eth0 inet static
	address 192.168.23.200
	netmask 255.255.255.0
	gateway 192.168.23.150
```

At last restart the networking service

	`sudo systemctl restart networking.service`

## On Windows

For Windows follow the given guide on the manual. Should be kinda the same.. 



## Check the accessibility of the gateway
![Milesight page.png](assets%2FMilesight%20page.png)

# Gateway Configuration
## WLAN

Change from AP to Client and connect it to your Router or Hotspot.

![WLAN Client.png](assets%2FWLAN%20Client.png)

Change WAN Failover from Cellular to WLAN:

![WLAN Failover.png](assets%2FWLAN%20Failover.png)

Under the maintainance tab you can now ping for instance 8.8.8.8 or something, and it should work if correctly setup.

## Packet Forwarder to TTN

Official Tutorial: [The Things Stack v3 Integration via Basic Station : IoT Support (milesight-iot.com)](https://support.milesight-iot.com/support/solutions/articles/73000514079-how-to-connect-milesight-gateway-to-the-things-stack-v3-via-basic-station)

In order to register the gateway in the TTN, the hardware address (1) and gateway ID (2) are required. Each ID can only be registered once in the TTN, so there is a chance that it has already been used by someone else, so change the default. Each gateway EUI (hardware address) can only be registered once in the Things Network at a time.
![Packet Forwarder Overview.png](assets%2FPacket%20Forwarder%20Overview.png)

The default packet forwarding must be turned off (3) and "Basic Station" is selected as port forwarding. The CA trust file is usually provided by the gateway manufacturer and can be downloaded here: https://letsencrypt.org/certs/isrgrootx1.pem (See official tutorial... jea I also don't trust it thaaat much ^^).
![Packet Forwarder.png](assets%2FPacket%20Forwarder.png)

The client key file must be generated in the TTN, whereby it should be noted that "traffic exchange" is selected.
![forwarding_key.png](assets%2Fforwarding_key.png)

Once added, the gateway should be available.
![added_gateway.png](assets%2Fadded_gateway.png)


# Disclaimer
 - I don't own any of these pictures
 - I hope this helped you at least a little bit ^^
 - Wish you a Merry Christmas