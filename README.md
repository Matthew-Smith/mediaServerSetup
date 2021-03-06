Set up home Media Server
======

Choosing an OS
------
I am sure there are plenty of reasons to use windows but because of its automatic updating and needing to be rebooted frequently I prefer to use linux. My distro of choice is Ubuntu/debian primarily due to familiarity and larger support for installation guides for the software I will be using but any distro can work!

Install a VPN
------
If you don't have access to a private torrent tracker, or you would like some extra layers of security a VPN is recommended. I personally use [Private Internet Acess](https://www.privateinternetaccess.com/) but there are many options out there.


##### Installation
First lets install OpenVPN
```shell
sudo apt-get update
sudo apt-get install openvpn
```

##### Configuration
Now download the configurations for OpenVPN
```shell
cd /etc/openvpn
sudo wget https://www.privateinternetaccess.com/openvpn/openvpn.zip
sudo unzip openvpn.zip
```

TODO: Test configurations (username & password required)

##### Auto Startup
TODO: set up OpenVPN as a service

##### Extra Security
TODO: set up internet kill switch (turn off internet if vpn stops)


Transmission
------
Transmission seems to be the easiest Torrent program to integrate with Sonnar/Radarr and if you do need to look at your downloads it is easy to do from any other computer on the network.

##### Installation
```
sudo apt-get install transmission
```

##### Set download file locations
TODO: Check what default state of these thigns are
TODO: fill out descriptions of what is happening
```shell
cd ~/.config/transmission
sudo service transmission-daemon stop
...
```

##### Set up unrar Script
This script will run automatically after the torrent finishes downloading and will extract the movie/tv show automatically
```shell
cd ~/.config/transmission
wget https://gist.githubusercontent.com/fmoledina/3bf792adb5a671448682969307c5e515/raw/f4a67b0293aca678ba6844b7ea264d3d0ece46e6/unrarer.sh
```

Now lets configure transmision's settings to execute the script when it is complete
```shell
cd ~/.config/transmission
sudo service transmission-daemon stop
vim settings.json
```

Then find these lines
```json
"script-torrent-done-enabled": false,
"script-torrent-done-filename": "",
```
And change them to 
```json
"script-torrent-done-enabled": true,
"script-torrent-done-filename": "/home/<user name>/.config/transmission/unrarer.sh",
``` 
Where `<user name>` is your user's name in linux

Restart the transmission daemon
```shell
sudo service transmission-daemon start
```

##### Enable Peer Accept
If 
![Transmission Accept port closed](https://raw.githubusercontent.com/Matthew-Smith/mediaServerSetup/master/transmission_port.png "Transmission accept port closed")

Jackett
------
This is used for adding ipTorrents or other private trackers to Sonarr/Radarr

##### Installation
TODO: Installation

##### Set up ipTorrents
TODO: explain how to get the cookie, this will need pictures

Sonarr
------
Sonarr is used for selecting TV shows to download as they become available. 

##### Installation
The instructions for installation can be found here:
https://github.com/Sonarr/Sonarr/wiki/Installation
After you start Sonnar with the `mono --debug /opt/NzbDrone/NzbDrone.exe`

##### Automatic Startup
Create the Sonarr init.d script
```shell
sudo touch /etc/systemd/system/sonarr.service
```

Paste this init script into the file
```INI
[Unit]
Description=Sonarr Daemon
After=network.target

[Service]
User=<your user name>
Group=<your user group>
Type=simple
ExecStart=/usr/bin/mono /opt/NzbDrone/NzbDrone.exe -nobrowser
TimeoutStopSec=20
KillMode=process
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
Be sure to change `<your user name>` and `<your user group>` 

When that is done execute 
```shell
sudo systemctl enable sonarr
```

and start the service
```shell
sudo service sonarr start
```

Radarr
------
Radarr is used for selecting and downloading movies as they become available. If you don't care to have an automated service for movies you can skip this

##### Installation
Here are the instructions I followed:
https://www.htpcguides.com/install-radarr-on-debian-8-jessie/


Plex
------
Plex is my media server of choice but other options include [DLNA](https://www.addictivetips.com/ubuntu-linux-tips/set-up-a-dlna-server-on-linux/), [Emby](https://emby.media/) and many others.

##### Installation
Here are the instructions I followed:
https://www.htpcguides.com/install-plex-media-server-ubuntu-16-x-and-later/

Organizr Landing Page
------
For this I use [organizr](https://organizr.us/), you can link all of the installed services into tabs and a home page which shows a heads up of what is going on for your server. 

##### Installation
TODO: installation instructions


##### Setup
TODO: Setup instructions
