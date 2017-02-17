## In this Guide

- [Connecting](getting-started-connect.md#connecting-to-the-edge)
- [Logging In](getting-started-connect.md#logging-in)
- [Connecting to WIFI](getting-started-connect.md#connecting-to-internet-via-wifi)

## In the Box

The BlueCats Edge kit contains:

- BlueCats Edge Bluetooth Scanner and Wireless Access Point
- Micro USB cable for power
- External Bluetooth antenna

## Connecting to the Edge

Start by plugging the supplied micro-usb cable into the edge and any USB port to power up the device and attach the external bluetooth antenna. Note that it may take 20-30 seconds to complete startup. The easiest way to configure the Edge is to connect it directly to your computer via Ethernet. Connect a straight-through ethernet cable between the ethernet port on your computer and the LAN port of the Edge. DHCP is enabled by default so with DHCP selected for your ethernet network adapter, your machine should be assigned an IP address in the range 192.168.8.0 for example:

![Connect to Edge using Ethernet](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/010-Connect.png "Connect with Ethernet")

Note the assigned IP address for later (in this case `192.168.8.147`) as this will be used as the test endpoint to receive scan data over UDP.

## Logging In

Each edge runs a local web configuration tool to manage both network configuration and BlueCats BLE scanning settings. After your computer has connected to the Edge over Ethernet, navigate to the Edge's IP address `192.168.8.1` using your web browser.

You will be prompted to log in with the root user credentials.

Username: `root`

Password is blank (unset) by default. After logging in without supplying a password the Web UI will prompt to configure one.

![Log in to Edge Web Configuration](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/Login.png "Log in")

## Edge Web Interface

Once logged in to the web configuration tool you will land on the System Overview screen. 

![Log in to Edge Web Configuration](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/SystemStatus.png "System Overview")

There are a considerable number of configuration menu items but only a couple will require any changes. Some of the additional useful features are listed at the end of this document under [Troubleshooting](#troubleshooting). All of the options to configure bluetooth scanning can be found under the 'BlueCats' main menu item.

![BlueCats Live View](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/LiveView.png "System Overview")

Optionally continue with WIFI setup or Start configuring Edge Applications

## Connecting to internet via WIFI

The Edge can be configured to send data to another machine on the local WIFI network, or connected to the internet to enable communication to the BlueCats Cloud or to download updated software.

Navigate to `Network` -> `Wireless` in the main menu and scan for available networks.

![Wireless](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/070-ConfigureWireless.png "Wireless")

Join an available network

![Join network](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/070-ConfigureWireless-2.png "Join network")

Enter network key and submit

![Enter key](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/070-ConfigureWireless-3.png "Enter key")

Save and apply changes

![Save and apply changes](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/070-ConfigureWireless-4.png "Save and apply")

Once connected to the local WIFI network, the Edge can be [configured to send to an IP address](#configuring-ble-scanner) on that network and the ethernet cable can then be disconnected as the Edge will continue sending data over the local WIFI connection while powered.

Now that your Edge is connected to your local machine or network, start configuring Edge Applications.
