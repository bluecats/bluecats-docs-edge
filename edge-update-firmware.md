### Updating BlueCats Edge Packages
#### Using web interface
There are two ways to update the BlueCats Edge packages to receive updates or after a firmware update. First is via the web GUI. This requires the Edge to be internet connected which you can do by configuring the WIFI as described in the getting started doc. Then you can navigate to System -> Software and then under the Actions tab there is an input box with 'Download and install package'. You can then paste each of the following links one at a time and hit 'OK'. This will download and install each of the packages.

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
