# onlineChecker

This script pings a device on your network and reports whether it is online or not to Domoticz by setting a virtual switch. In a wider context this can then be used to detect the presence of someone or some activity in your house (e.g. am I home, is the laptop or tv on, etc.)

## Installation

- Navigate to your Domoticz' scripts folder (mine's `~/domoticz/scripts/`) and clone this repository into that directory:
`git clone https://github.com/jorijnsmit/onlineChecker.git`

- onlineChecker depends on the [`requests` library](http://docs.python-requests.org/en/master/): `pip install requests`

- You might also need to install [`configparser`](https://docs.python.org/3/library/configparser.html): `pip install configparser`

- Copy `config.template.ini` to `config.ini` and edit the file so that it contains your Domoticz details.

- Copy `devices/example.ini` for every device, like your TV; `devices/tv.ini` and edit the file so that it contains your Device details.

You can now run the script!

If you want to have multiple devices, you can specify which one to check by giving its device-name as an argument in the command-line: `python online_checker.py tv`


## Run as a systemd service

To help manage when and how the script runs, you can install it as a `systemd` service. This will make it very easy to always run in the background, restart automatically, run on startup, log properly etc. You can learn more about systemd [here](https://wiki.debian.org/systemd).

A simple `.service` file to manage this script goes into `/etc/systemd/system/` and could look like this:

```
[Unit]
Description=onlineChecker

[Service]
Type=simple
ExecStart=/usr/bin/python3 -u online_checker.py
WorkingDirectory=/home/jorijnsmit/domoticz/scripts/onlineChecker
User=jorijnsmit
Restart=always

[Install]
WantedBy=multi-user.target
```
Running `python` unbuffered (`-u`) makes sure the print messages don't get buffered but go straight to the console/log.

Once that file is in place you can enable/disable, start/stop etc. it using `systemctl` commands.

That's all!

## Credits

All credits go to "Chopper_Rob" for the core logic of this script, as I found it published on https://www.domoticz.com/wiki/Presence_detection (original url is unfortunately offline).
