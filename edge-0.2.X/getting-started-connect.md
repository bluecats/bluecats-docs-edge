## In this Guide

- [Connecting](getting-started-connect.md#connecting-to-the-edge)
- [Logging In](getting-started-connect.md#logging-in)
- [Connecting to WiFi](getting-started-connect.md#connecting-to-internet-via-wifi)
- [Registering the device](getting-started-connect.md#register-with-bluecats-cloud)

## In the Box

The BlueCats Edge kit contains:

- BlueCats Edge Bluetooth Scanner and Wireless Access Point
- Micro USB cable for power
- External Bluetooth antenna

## Connecting to the Edge (With Ethernet)
The easiest way to configure the Edge is to connect it directly to your computer via Ethernet. 

1. Connect an ethernet cable to your computer and to the LAN port of the Edge.
2. Attach the external bluetooth antenna. 
3. Plug in the  supplied micro-usb cable into the edge to power up the device. (Note that it may take 20-30 seconds to complete startup.)
4. Then go to this address [192.168.8.1](http://192.168.8.1).

## Connecting to the Edge Through WiFi (Without Ethernet)
Start by plugging the supplied micro-usb cable into the edge and any USB port to power up the device and attach the external bluetooth antenna. Note that it may take 20-30 seconds to complete startup. 

1. Look at the bottom of your Edge. There should be an SSID (i.e. bluecats-230).
3. Then look in your WiFi connections and find that WiFi Name. 
4. Connect to it. 
5. Then go to this address [192.168.8.1](http://192.168.8.1).


## Sending your Data (Protocols and Formats)
For sending data to your endpoints. 
We currently support the data formats: 
* JSON 
* CSV 
* binary-format

We support the protocols: 
* MQTT
* UDP
* HTTP

## Logging In

Each edge runs a local web configuration tool to manage both network configuration and BlueCats BLE scanning settings. After your computer has connected to the Edge over Ethernet, navigate to the Edge's IP address [192.168.8.1](http://192.168.8.1) using your web browser.

You will be prompted to log in with the root user credentials.

Username: `root`

Password is blank (unset) by default. After logging in without supplying a password, the Web UI will prompt to configure one.

![Log in to Edge Web Configuration](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/Login.png "Log in")

## Edge Web Interface / Checking your Firmware Version

Once logged in to the web configuration tool you will land on the System Overview screen. Here you can see what Edge firmware version. You are on and this allows you to check which documenation to follow. If you need to change to documentation, here is the landing page for all of the [Edge documentation](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/EdgeMainView.png). 

![Log in to Edge Web Configuration](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/EdgeMainView.png "System Overview")

There are a considerable number of configuration menu items but only a couple will require any changes. Some of the additional useful features are listed at the end of this document under [Troubleshooting](#troubleshooting). All of the options to configure bluetooth scanning can be found under the 'BlueCats' main menu item.

![BlueCats Live View](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/EdgeRegisterBlueCatsCloud.png "System Overview")

[Start Configuring Edge Applications](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/EdgeUIBlueCatsMenu.png) or continue below by connecting your Edge using the WiFi Setup.

## Connecting to Internet via WiFi

The Edge can be configured to send data to another machine on the local WiFi network, or connected to the internet to enable communication to the BlueCats Cloud or to download updated software.

Navigate to `Network` -> `Wireless` in the main menu and scan for available networks.

![Wireless](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/070-Configure-Wireless.png "Wireless")

Join an available network

![Join network](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/070-Configure-Wireless-2.png "Join network")

Enter network key and submit

![Enter key](https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/EdgeConnectingWiFi.png "Enter key")

Save and apply changes

![Save and apply changes](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/070-Configure-Wireless-4.png "Save and apply")

Once connected to the local WiFi network, the Edge can be [configured to send to an IP address](#configuring-ble-scanner) on that network and the ethernet cable can then be disconnected as the Edge will continue sending data over the local WiFi connection while powered.

## Register with BlueCats Cloud

By registering the edge device with BlueCats cloud, you can monitor uptime and request the latest available firmware. 

Navigate to `BlueCats` -> `Connect and Manage`. If the 'Device Register Status' is 'Registered' then the edge device is successfully connected with the BlueCats cloud services. 
  <p align="center"><img width="500px" src="https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/EdgeRegisterBlueCatsCloud.png"/></p>

If the status is 'Invalid', then follow the steps below to register the device.

  - If you have your Edge, but don't yet have a BlueCats account follow these steps to create an account and [claim your devices](http://support.bluecats.com/customer/portal/articles/2345533-what-is-a-claim-code-)
  - Log into [https://app.bluecats.com/devices](https://app.bluecats.com/devices)
  <p align="center"><img width="500px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/bluecats-login.png" alt="bluecats-login"/></p>
  
  - Find the Edge you are updating under Devices (you can search by the serial number printed on the label of each Edge) and view its details.
  <p align="center"><img width="600px" src="https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/BlueCatsAppDevices.png"/></p>

  - Click the 'Revoke Access Token' button.
  <p align="center"><img width="600px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/bluecats-revoke.png" alt="bluecats-revoke"/></p>

  - Go back to the Edge UI and click the 'Re-Register' button.
  <p align="center"><img width="600px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/edge-re-register.png" alt="edge-register"/></p>
  
  - You can test the connectivity to BlueCats Cloud by clicking the 'Test Connection' button. The status will be 'Unknown' when there is no connection and 'Unauthorized' when not registerd. 

Now that your Edge is connected to your local machine or network, you can [update edge firmware](edge-update-firmware.md), [update BLE module firmware](edge-update-bluetooth-firmware.md) and start configuring [Edge Applications](getting-started-edge-applications.md#bluecats-edge-applications---overview).



