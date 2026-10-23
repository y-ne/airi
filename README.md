# Airi, the Army Ant

**NOTES** : For now, I'll just do some research on my board and then try to take precautions to avoid bricking my only device that I have lying around.

### SPECS

```
SoC : Qualcomm Snapdragon 410 (MSM8916)
RAM : 512M LPDDR3
Storage : 4G eMMC
Cellular : Integrated 4G LTE Cat-4
Wireless : 802.11b/g/n 2.4 GHz + Bluetooth 4.0
```

```bash
# PCB Silk
# FY_UZ801_V3.0

adb devices -l
# List of devices attached
#0123456789ABCDEF       device product:msm8916_32_512 model:UZ801 device:msm8916_32_512 transport_id:3
```

### SITES

this is the sites i'm refering to

https://openstick.org/boards/uz801/flashing/
https://wvthoog.nl/openstick/

### TOOLS

I'm using Ubuntu 26 so this is what we need

Packages

```
# ADB and Fastboot
android-tools-adb
android-tools-fastboot

# EDL
adb
fastboot
python3-dev
python3-pip
liblzma-dev
git
```
EDL

```bash
sudo systemctl stop ModemManager
sudo systemctl disable ModemManager

git clone https://github.com/bkerler/edl.git
cd edl
git submodule update --init --recursive
python3 -m venv .venv
. .venv/bin/activate
pip install -r requirements.txt
```

### BACKUP

```bash

```
