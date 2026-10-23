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

# EDL (purge)
modemmanager
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

UDEV Rules

```bash
sudo tee /etc/udev/rules.d/51-android-msm.rules << 'EOF'
SUBSYSTEM=="usb", ATTR{idVendor}=="05c6", ATTR{idProduct}=="9008", MODE="0666", GROUP="plugdev"
SUBSYSTEM=="usb", ATTR{idVendor}=="05c6", ATTR{idProduct}=="9091", MODE="0666", GROUP="plugdev"
SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="d00d", MODE="0666", GROUP="plugdev"
EOF

sudo udevadm control --reload-rules
sudo udevadm trigger
```

### BACKUP

```bash
# DUMP Partition with EDL

# Primary
python3 edl.py r gpt gpt_backup.bin

# Modem Calibration
python3 edl.py r modemst1 modemst1.bin
python3 edl.py r modemst2 modemst2.bin
python3 edl.py r fsg fsg.bin
python3 edl.py r fsc fsc.bin

# Boot & Recovery
python3 edl.py r boot boot_stock.bin
python3 edl.py r aboot aboot_stock.bin

# full eMMC
python3 edl.py rl my_dongle_full_dump/
```

### Restore (if cellular fail)

put modem on EDL Mode

```bash
python3 edl.py w modemst1 modemst1.bin
python3 edl.py w modemst2 modemst2.bin
python3 edl.py w fsg fsg.bin
python3 edl.py w fsc fsc.bin
python3 edl.py reset
```

### Firmware

Firmware based on `Wim van 't Hoog` 's Debian Bookworm build.

https://download.wvthoog.nl/projects/security/openstick/openstick-uz801-v3.0.zip

then just ran the `./flash.sh` command to flash it onto eMMC.

### Credentials

Username : user

Password : 1

RNDIS : 192.168.200.1

WiFi : 192.168.100.1

### Connect to WLAN

```bash
nmcli connection delete hotspot
sudo nmcli dev wifi connect "SSID" password "PASSWORD"

# Check if connected
ip show a wlan0
```
