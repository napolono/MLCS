# Building a Mini Local Communication System with Raspberry Pi and Mumble

## Introduction

Welcome to the exciting journey of creating your very own mini local communication system! This project leverages the power of a Raspberry Pi and the flexibility of the Mumble server to enable seamless voice communication across multiple devices. Whether you're organizing an event, coordinating a small business, or simply want a robust intercom system for your home, this guide will walk you through the entire process. Let's dive in!

## Why Build This System?

Imagine a world where you can:
- Effortlessly communicate with team members across different rooms or buildings.
- Coordinate activities and share updates in real-time during events or emergencies.
- Enable interactive learning sessions in educational institutions.
- Enhance your gaming experience with uninterrupted voice chat.
- Maintain seamless communication in remote work sites or outdoor events.

With this system, all these scenarios become possible without relying on external internet connections. Let's get started!

## What You'll Need

 - A Raspberry Pi (Minimum: Pi Zero W, Recommended: Pi 3 or 4)
 - Wifi USB Adapter (Optional, For Better Connectivity)
 - SD Card (Minimum: 8GB, Recommended: 16GB Or Higher)
 - Computer To Install the OS Linux Distro Image Compatible With The Raspberry Pi (Recommended: Raspbian)
 - Smartphones As Clients (Android or IOS)
 - Power Bank (Optional, For Portability)


## Step-by-Step Tutorial

### Step 1: Prepare Your Raspberry Pi

1. **Install Raspbian OS**:
   Make sure your Raspberry Pi is running the latest version of Raspbian OS. You can download it from the [official website](https://www.raspberrypi.org/software/).

2. **Update and Upgrade:**
   Open a terminal and run:
   ```bash
   sudo apt update
   sudo apt upgrade
   ```
### Step 2: Install and Configure the Mumble Server
1. **Install Murmur:**
   ```bash
   sudo apt install mumble-server
2. **Configure Murmur:** Edit the configuration file:
   ```bash
   sudo nano /etc/mumble-server.ini
   ```
   Set the 'host' parameter to your desired IP address, for example:
   ```
   host=192.168.4.1
   ```
3. **Start the Mumble Server:**
   ```bash
   sudo systemctl enable mumble-server
   sudo systemctl start mumble-server
   ```
### Step 3: Configure Wi-Fi Access Points
1. **Install Necessary Packages:**
   ```bash
   sudo apt install hostapd dnsmasq
   ```
2. **Configure wlan0 (AP1):**
   Edit ``/etc/dhcpcd.conf``
   ```
   interface wlan0
   static ip_address=192.168.4.1/24
   nohook wpa_supplicant
   ```
   Configure ``/etc/hostapd/hostapd.conf``:
   ```
   interface=wlan0
   driver=nl80211
   ssid=AP1
   hw_mode=g
   channel=7
   wmm_enabled=0
   macaddr_acl=0
   auth_algs=1
   ignore_broadcast_ssid=0
   wpa=2
   wpa_passphrase=your_password
   wpa_key_mgmt=WPA-PSK
   rsn_pairwise=CCMP
   ```

3. **Start Hostapd:**
   Edit ``/etc/defualt/hostapd``:
   ```
   DAEMON_CONF="/etc/hostapd/hostapd.conf"
   ```
   Enable and start the service:
   ```bash
   sudo systemctl unmask hostapd
   sudo systemctl enable hostapd
   sudo systemctl start hostapd
   ```
### Step 4: Configure DNS and DHCP
1. **Configure dnsmasq:**
Edit ``/etc/dnsmasq.conf``
   ```
   interface=wlan0
   dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h
   ```
2. **Restart dnsmasq:**
   ```bash
   sudo systemctl restart dnsmasq
   ```
### Step 5: Test Your Setup
1. **Connect Devices**
Connect your devices to `AP1` using the SSID and password you configured.
2. **Install Mumble Client:**
Download and install the Mumble client on your devices.
3. **Configure Mumble Client**
Set the server IP address to `192.168.4.1` and connect.
4. **Test Communication:**
Start a voice call and test the connection 