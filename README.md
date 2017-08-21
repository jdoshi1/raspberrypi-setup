raspberrypi-setup
====

*Note: The following instructions are tested on Mac*

Table of Contents
=================

* [raspberrypi\-setup](#raspberrypi-setup)
  * [Faster login to your raspberry pi machine](#faster-login-to-your-raspberry-pi-machine)

## Faster login to your raspberry pi machine
*login to raspberrypi without password*

1. create a ssh-key
```bash
ssh-keygen -t rsa -C "your_email@example.com"
```
  - Just press <Enter> to accept the default location and file name. If the .ssh directory doesn't exist, the system creates one for you.
  - Enter, and re-enter, a passphrase when prompted (You can keep it blank if you are lazy like I am)
  - Your ssh public-private key pair will be generated here --> ```ls -al ~/.ssh/id_rsa```

2. Copy your ssh public key in the RPi
```bash
pbcopy < ~/.ssh/id_rsa.pub # Make sure the public key file is correct
ssh pi@raspberrypi.local # This is the only time you will need to enter the password
echo "paste_your_key_surrounded_by_these_quotes" >> ~/.ssh/authorized_keys
```
  - Now logout and login to see you dont need a password
