## In this Guide

- [Connecting](https://github.com/bluecats/bluecats-docs-edge/blob/master/getting-started-connect.md#connecting-to-the-edge)
- [Logging In](https://github.com/bluecats/bluecats-docs-edge/blob/master/getting-started-connect.md#logging-in)
- [Connecting to WIFI](https://github.com/bluecats/bluecats-docs-edge/blob/master/getting-started-connect.md#connecting-to-internet-via-wifi)
- [Troubleshooting](https://github.com/bluecats/bluecats-docs-edge/blob/master/getting-started-connect.md#troubleshooting)
  - [View Logs](https://github.com/bluecats/bluecats-docs-edge/blob/master/getting-started-connect.md#viewing-local-logs)
  - [Reset Edge to Default Firmware](https://github.com/bluecats/bluecats-docs-edge/blob/master/getting-started-connect.md#resetting-edge-to-default-firmware-when-locked-out)
  - [Update BlueCats Packages](https://github.com/bluecats/bluecats-docs-edge/blob/master/getting-started-connect.md#updating-bluecats-edge-packages)

## In the Box

The BlueCats Edge kit contains:

- BlueCats Edge Bluetooth Scanner and Wireless Access Point
- Micro USB cable for power
- External Bluetooth antenna

## Connecting to the Edge

Start by plugging the supplied micro-usb cable into the edge and any USB port to power up the device and attach the external bluetooth antenna. Note that it may take 20-30 seconds to complete startup. The easiest way to configure the Edge is to connect it directly to your computer via Ethernet. Connect a straight-through ethernet cable betwen the ethernet port on your computer and the WAN port of the Edge. DHCP is enabled by default so with DHCP selected for your ethernet network adapter, your machine should be assigned an IP address in the range 192.168.8.0 for example:

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

Navigate to `Network` -> `Wireless` in the main menu and the scan for available networks

![Wireless](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/070-ConfigureWireless.png "Wireless")

Join an available network

![Join network](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/070-ConfigureWireless-2.png "Join network")

Enter network key and submit

![Enter key](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/070-ConfigureWireless-3.png "Enter key")

Save and apply changes

![Save and apply changes](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/070-ConfigureWireless-4.png "Save and apply")

Once connected to the local WIFI network, the Edge can be [configured to send to an IP address](#configuring-ble-scanner) on that network and the ethernet cable can then be disconnected as the Edge will continue sending data over the local WIFI connection while powered.

Now that your Edge is connected to your location machine or network start configuring Edge Applications

## Troubleshooting

### DHCP issues when connecting to multiple Edges using Ethernet

When swapping between multiple Edges using a local Ethernet connection to your computer (in this case a MacBook running OSX) you may encounter DHCP issues with network errors such as 'Unable to assign IP' when attempting to connect to the second Edge. One work around is to reset the local DHCP client. This can be done in OSX via System Preferences -> Network, by first switching to BOOTP (and apply) and then switch back to DHCP (and apply). The Edge may also need to be rebooted to successfully assign an IP address.

Change to BOOTP and Apply (this is just to allow us to switch back to DHCP)
![SET BOOTP](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/Troubleshoot-BOOTP.png "Set BootP")

Change back to DHCP and Apply
![Set DHCP](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/Troubleshoot-DHCP.png "Set DHCP")

Reboot the Edge and an IP address should be successfully assigned to your computer.

The above can also be achieved via Terminal:

Run `ifconfig` command in Terminal to determine active ethernet interface, in this case 'en5'
Run the following in terminal, where 'en5' is the ethernet interface:
`sudo ipconfig set en5 DHCP`

### Viewing local logs

The local device logs can be used to see if beacons are being detected and if they are being successfully sent to the configured endpoint. For example the following logs show that the UDP endpoint is not available:

![View local logs](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/050-View-Logs.png "View Logs")

### Resetting Edge To Default Firmware When Locked Out
1. Connect edge WAN into your computer with ethernet cable
2. Unplug edge power
3. Hold Reset
4. Plug in power while holding reset. Edge will flash some LEDs, and then start blinking red LED. Let red LED blink 6 times, then let go of reset. You should see the red LED blink fast for 1 sec.
5. Disable your wifi, and set your own ip to 192.168.1.2 for your ethernet interface
![Network Settings](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/network-settings.jpg "Network Settings")
6. Go to 192.168.1.1 in your web browser
7. Get our firmware, upload and apply it
![Upload Firmware](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/upload-firmware-pic.jpg "Upload Firmware")
8. Wait a minute. Set your ip to something like 192.168.8.118 (just not 192.168.8.1) for the ethernet interface and go to 192.168.8.1 in your browser You should see the Bluecats OpenWrt web interface

### Updating BlueCats Edge Packages
#### Using web interface
There are two ways to update the BlueCats Edge packages to receive updates or after a firmware update. First is via the web GUI. This requires the Edge to be internet connected which you can do by configuring the WIFI as described in the getting started doc. Then you can navigate to System -> Software and then under the Action tab there is an input box with 'Download and install package'. You can then paste each of the following links one at a time and hit 'OK'. This will download and install each of the packages.

Contact support@bluecats.com for download urls for latest packages.

Note that this method only supports download via HTTP.

![Upload Package](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/edge-update-package.png "Upload Package")

#### Using SSH

Download the package files to your local computer:

Use secure copy to transfer to the Edge:

    scp path-to-your-files/luci-base.ipk root@192.168.8.1:~
    scp path-to-your-files/luci-app-bluecats.ipk root@192.168.8.1:~

SSH into the Edge:

    ssh root@192.168.8.1

Then install the packages:

    opkg install luci-base.ipk
    opkg install luci-app-bluecats.ipk
