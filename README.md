![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Platform](https://img.shields.io/badge/platform-Raspberry%20Pi-red.svg)
![ASL3](https://img.shields.io/badge/ASL-3.0-blue.svg)
![Shell Script](https://img.shields.io/badge/shell-bash-green.svg)


# Quick Start Guide for Enabling APRS on Your ASL3 Raspberry Pi Node

This guide will help you set up the APRS your AllStarLink 3

## What is APRS?

APRS stands for Automatic Packet Reporting System. It’s a digital communication system used in amateur radio to share real-time data over radio frequencies.

📡 What APRS actually does

Instead of voice, APRS sends short data packets that can include:

- 📍 GPS position (your location on a map)
- 💬 Short text messages
- 🚗 Vehicle tracking (cars, bikes, balloons)
- 🌦️ Weather station data
- 📊 Telemetry (voltage, sensors, etc.)

Visit this link to find out more: https://www.aprs.org/
To view aprs map go to: https://aprs.fi/

## Prerequisites

Before you begin, ensure you have:
- ✓ Raspberry Pi running AllStarLink 3 (ASL3)
- ✓ SSH access to your Raspberry Pi
- ✓ Your node is configured and working
- ✓ Internet connection on your Raspberry Pi
- ✓ A Valid Ham Callsign and APRS Passcode go to this link to generate one: https://apps.magicbug.co.uk/passcode/

### Step 1: SSH to your Raspberry Pi

SSH into your Raspberry Pi and create a backup of your gps.conf file:

```bash
cd ~
cd /etc/asterisk/
cp gps.conf gps.conf.bak
```

### Step 2: Edit your gps.conf file

Use the template below, edit you callsign, passcode and lat/long.

```bash

[general]
call = CALLSIGN                ; Your callsign and SSID (e.g., -10 is IGate/Node)
password = 1234                ; Your APRS-IS passcode
comment =                      ; Desciption of your Mode text displayed on APRS maps e.g. _Allstarlink 487088 RF Bridge 432.70 Mhz_
server = aunz.aprs2.net        ; APRS-IS server
port = 14580                   ; Standard APRS-IS port
icon = -                       ; "r" is the standard icon for a repeater/node
lat = -36.9711623005192        ; Your latitude in decimal degrees
lon = 174.90249871978133       ; Your longitude in decimal degrees
elev = 20                     ; Elevation in meters
interval = 600                 ; Beacon interval in seconds (600 = 10 mins)

```

### Step 3: Enable GPS module in rpt.conf

While still on the same asterisk directory edit rpt.conf and make sure shared object module (app_gps.so )on your rpt.conf is loaded.

```bash
nano rpt.conf
```

**Locate gps module which is on the [modules] stanza**

```bash
noload     = app_gps.so                     ; GPS Interface
```

### Step 4: Change noload to load

```bash
load     = app_gps.so                     ; GPS Interface
```

### Step 5: Save the new config and reload the module

CTRL + X then YES to Save

### Step 6: Reload all modules from shell

```bash
asterisk -rx "module reload"
```

### Step 7:Restart Asterisk

```bash
systemctl restart asterisk
```
