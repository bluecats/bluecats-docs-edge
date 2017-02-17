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
1. Connect edge LAN into your computer with ethernet cable
2. Unplug edge power
3. Hold Reset
4. Plug in power while holding reset. Edge will flash some LEDs, and then start blinking red LED. Let red LED blink 6 times, then let go of reset. You should see the red LED blink fast for 1 sec.
5. Disable your wifi, and set your own ip to 192.168.1.2 for your ethernet interface
![Network Settings](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/network-settings.jpg "Network Settings")
6. Go to 192.168.1.1 in your web browser
7. Get our firmware, upload and apply it
![Upload Firmware](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/upload-firmware-pic.jpg "Upload Firmware")
8. Wait a minute. Set your ip to something like 192.168.8.118 (just not 192.168.8.1) for the ethernet interface and go to 192.168.8.1 in your browser You should see the Bluecats OpenWrt web interface
