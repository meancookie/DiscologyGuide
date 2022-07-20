# Setup DHCP Server
# macOS
1. Config local wired network interface card to static IP
- Open Network Preferences
- IP address - 10.0.0.1
- Subnet Mask - 255.255.255.0
- Router - 10.0.0.1

2. Create DHCP Server config file: bootpd.plist
- Open Terminal application and execute script as following
``` shell
sudo vi /etc/bootpd.plist
```
- To check the right network interface card id. Open Terminal application and execute script as following.
  choose the line with ip same as 10.0.0.1
```
ifconfig
```


- Copy this content to bootpd.plist file, ensure the right network interface card id.
``` xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">

<plist version="1.0">

<dict>

    <key>bootp_enabled</key>

    <false/>

    <key>detect_other_dhcp_server</key>

    <integer>1</integer>

    <key>dhcp_enabled</key>

    <array>

        <string>en0</string>

    </array>

    <key>reply_threshold_seconds</key>

    <integer>0</integer>

    <key>Subnets</key>

    <array>

        <dict>

            <key>allocate</key>

            <true/>

            <key>dhcp_router</key>

            <string>10.0.0.1</string>

            <key>lease_max</key>

            <integer>86400</integer>

            <key>lease_min</key>

            <integer>86400</integer>

            <key>name</key>

            <string>10.0.0</string>

            <key>net_address</key>

            <string>10.0.0.0</string>

            <key>net_mask</key>

            <string>255.255.255.0</string>

            <key>net_range</key>

            <array>

                <string>10.0.0.5</string>

                <string>10.0.0.250</string>

            </array>

        </dict>

    </array>

</dict>

</plist>
```

To save a file and quit in Vim, press ```Esc``` key, type ```:wq``` and hit Enter key.

4. Start DHCP Server
``` shell
sudo /bin/launchctl load -w /System/Library/LaunchDaemons/bootps.plist
```
5. Stop DHCP Server
``` shell
sudo /bin/launchctl unload -w /System/Library/LaunchDaemons/bootps.plist
```
## Requirements
- maxOS 10+


# Setup Unifi Network Application
## Requirements
- [Unifi Network Application](https://www.ui.com/download-software/)
- Switch Enterprise 8 PoE (version: [6.2.14.13855](https://www.ui.com/download/unifi-switching-routing/unifi-switch-enterprise-8-poe){:target="_blank"})
- Access Point FlexHD (version: [6.0.21.13673](https://www.ui.com/download/unifi/unifi-flex-hd)) ***optional***
- Access Point AC Mesh Pro ***optional***
- [How to upgrade unifi device firmware](https://www.youtube.com/watch?v=Rgh677uSEd4)

## Wiring
- Notebook connect to Switch POE 1
- Access Point connect to Switch POE 2

## Method 1: Restore in the UniFi Startup Wizard
If beginning a new installation, it will be easier to just use the option of restore from a previous backup as soon as the UniFi Startup Wizard launches, and select your [.unf](https://github.com/meancookie/discology/files/9119795/net_backup_07-15-2022_v7.1.66.unf.zip) file.


## Method 2: Setup Vlan and Multicast manually
### Create Vlan
1. Go to **Settings > Networks** and click the **Create New Network** button.
2. Fill the following settings:
  - Network Name: discology vlan
  - Router: Select the **USW-Enterprise-8-POE** on the **Router** drop-down field.
  - Gateway IP/Subnet: Uncheck the **Auto Scale Network** then fill the **10.10.0.1** to the **Host Address**, select the **16 (65489 usable hosts)** on the **Netmask** drop-down field.
  - Advanced Configuration: Check **Manual** option.
  - Vlan ID: 10
3. Click the **Add Network** Button.
<img width="1440" alt="add vlan network" src="https://user-images.githubusercontent.com/3402605/178220888-dae97f7d-0714-4f2e-9cfb-9919cadd54a7.png">
<img width="1440" alt="manual confiugration" src="https://user-images.githubusercontent.com/3402605/178221052-672675bf-f74f-498c-9066-1d10d133022a.png">

### Create Wifi
1. Go to **Settings > WiFi** and click the **Create New WiFi Network** button.
2. Fill the following settings:
  - Name: discology vlan
  - Password: ******
  - Network: Select the **discology vlan** on the **Network** drop-down field.
  - Advanced Configuration: Check **Manual** option.
  - Multicast Enhancement: Check **Enable** option.
3. Click the **Add WiFi Network** Button.
<img width="1431" alt="add wifi network" src="https://user-images.githubusercontent.com/3402605/178222850-d9ccfa48-8454-4c12-b230-6b5b1cde5698.png">
<img width="1425" alt="manual configuration" src="https://user-images.githubusercontent.com/3402605/178222873-d8cfcf6c-71ab-44bb-ae87-bc8861f58e66.png">

### WiFi Configuration
1. Go to **Settings > WiFi** and under **Global AP Settings > AP Site Settings**.
2. Wireless Meshing: Uncheck **Enable** option.
3. Click the **Apply Changes** Button.

### Device Static IP
#### Switch
1. Go to **UNIFI Devices** and click the **SWITCH** which you are using.
2. On the right slider section and go to **Settings** section.
3. Under **Network** field.
   - Configure IP: Select the **Static IP** on the **Configure IP** drop-down field.
   - IP Address: 10.0.0.2
   - Subnet Mask: 255.255.255.0
   - Gateway: 10.0.0.2
   - Preferred DNS: 10.0.0.2
4. Click the **Apply Changes** Button.
<img width="1440" alt="Switch Static IP" src="https://user-images.githubusercontent.com/3402605/178228348-52227231-4871-464e-893c-5d01c38193bd.png">

#### AP
1. Go to **UNIFI Devices** and click the **AP** which you are using.
2. On the right slider section and go to **Settings** section.
3. Under **Network** field.
   - Configure IP: Select the **Static IP** on the **Configure IP** drop-down field.
   - IP Address: 10.0.0.3
   - Subnet Mask: 255.255.255.0
   - Gateway: 10.0.0.2
   - Preferred DNS: 10.0.0.2
4. Click the **Apply Changes** Button.
<img width="1434" alt="AP Static IP" src="https://user-images.githubusercontent.com/3402605/178230048-a2bd7d79-595a-49f5-8ed8-c799a1537c60.png">

### Host apply VLAN
1. Go to **UNIFI Devices** and click the **Switch** which you are using.
2. On the right slider section and go to **Ports** section.
3. Click **Port Number** which your **Host Device** connect to.
5. Port Profile: Select the **discology vlan** on the **Port Profile** drop-down field.
6. Click the **Apply Changes** Button.
<img width="1436" alt="Screen Shot 2022-07-11 at 17 44 22" src="https://user-images.githubusercontent.com/3402605/178236943-7d908cee-1078-4b0e-9a7e-5a783fd5a108.png">

## Purge Unifi Network Application Site Data
1. Delete the UniFi Network application from the Applications section.
2. Delete the folder: ~/Library/Application Support/UniFi/

## Additional Info: on updating firmware version
https://www.ui.com/download/unifi-switching-routing/unifi-switch-enterprise-8-poe

```
scp US.mvpj4b_6.2.14+13855.220408.1447.bin Discologynow@10.0.0.2:/tmp/fwupdate.bin
```

**fwupdate.bin**

```
Cd /tmp
syswrapper.sh upgrade2 &
```
