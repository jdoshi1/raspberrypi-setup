raspberrypi-setup
====

*Note: The following instructions are tested on Mac*

Table of Contents
=================

* [raspberrypi\-setup](#raspberrypi-setup)
  * [Passwordless login to your raspberry pi machine](#passwordless-login-to-your-raspberry-pi-machine)
  * [Setup multiple wifi networks](#setup-multiple-wifi-networks)

## Passwordless login to your raspberry pi machine
*login to raspberrypi without password*

1. create a ssh-key
```bash
ssh-keygen -t rsa -C "your_email@example.com"
```
  - Just press \<Enter\> to accept the default location and file name. If the .ssh directory doesn't exist, the system creates one for you.
  - Enter, and re-enter, a passphrase when prompted (You can keep it blank if you are lazy like I am)
  - Your ssh public-private key pair will be generated here --> ```ls -al ~/.ssh/id_rsa```

2. Copy your ssh public key in the RPi
```bash
pbcopy < ~/.ssh/id_rsa.pub # Make sure the public key file is correct
ssh pi@raspberrypi.local # This is the only time you will need to enter the password
echo "paste_your_copied_public_key_surrounded_by_these_quotes" >> ~/.ssh/authorized_keys
```
  - Now you can login without a password

## Setup multiple wifi networks

*Reference: https://www.mikestreety.co.uk/blog/use-a-raspberry-pi-with-multiple-wifi-networks*

```bash
sudo nano /etc/network/interfaces
```

and add the following lines to the file:
```
auto lo
iface lo inet loopback
auto eth0
allow-hotplug eth0
iface eth0 inet dhcp
auto wlan0
allow-hotplug wlan0
iface wlan0 inet manual
wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
iface home inet dhcp
iface office inet dhcp
```

- Now edit the wpa_supplicant.conf file

```bash
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

It should look something like this
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
    ssid="WIFI SSID"
    psk="WIFI PASSWORD"
    id_str="home"
}
network={
    ssid="WIFI SSID"
    psk="WIFI PASSWORD"
    id_str="office"
}
```

*Note: Make sure you are adding connection to 2.4 and 5 GHz both; since RPi has known to not work for 5GHz*

- Now reboot your rpi
```bash
sudo reboot
```
