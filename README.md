# Waydroid Guide

This guide takes you through the process of installing waydroid, and getting arm translation on Linux.

### System details:
```

OS: Zorin OS 16 x86_64
Kernel: 5.11.0-41-generic
DE: GNOME
WM: Mutter
Terminal: gnome-terminal
CPU: Intel i3-7020U (4) @ 2.300GHz
GPU: Intel HD Graphics 620
Memory: 3812MiB
```

## Pre-requisites:

1. Follow the pre-requisites section: https://docs.waydro.id/usage/install-on-desktops#install-pre-requisites

2. Download lineageOS system images archive from Google Drive: https://drive.google.com/file/d/1sXxng8ALWEK3Yjg4BVvU_Xy1NJcUdFV1/view?usp=drivesdk

3. Create the system images directory:
```
sudo mkdir -p /usr/share/waydroid-extra/images
```

4. Extract the contents of the archive to ```~/temp_folder``` (Where ~ denotes the user home directory)

5. Move the contents of the ```temp_folder``` to system images directory:
```
sudo mv temp_folder/* /usr/share/waydroid-extra/images
rmdir temp_folder
```

6. Get the latest libgbinder updates:
```
sudo apt install git libglib2.0-dev libglibutil-dev
git clone --depth=1 https://github.com/mer-hybris/libgbinder
cd libgbinder
make
make install
```

7. Copy the new ```libgbinder.so.1```, ```libgbinder.so.1.1``` and ```libgbinder.so.1.1.14``` files added to /usr/lib to wherever your system has the current libgbinder installed. Use ```dpkg -L libgbinder``` to find out.

## Install Waydroid

1. Follow the install waydroid section : https://docs.waydro.id/usage/install-on-desktops#install-waydroid

2. Edit ```sudo nano /etc/gbinder.d/anbox.conf``` to read like:
```
[Protocol]
/dev/anbox-binder = aidl3
/dev/anbox-vndbinder = aidl3
/dev/anbox-hwbinder = hidl

[ServiceManager]
/dev/anbox-binder = aidl3
/dev/anbox-vndbinder = aidl3
/dev/anbox-hwbinder = hidl

[General]
ApiLevel = 30
```

3. Restart waydroid:
```
sudo systemctl restart waydroid-container.service
waydroid show-full-ui
```

## Arm Translation

1. Install lzip:
```
sudo apt install lzip
```

2. Install Waydroid Extras Script:
```
git clone --depth=1 https://github.com/casualsnek/waydroid_script
cd waydroid_script
sudo python3 -m pip install -r requirements.txt
sudo python3 waydroid_extras.py -h
```

3. Install Libhoudini arm Translation:
```
cd waydroid_script
sudo python3 waydroid_extras.py -l
```

You may need to ```umount /dev/loop12`` and ```waydroid session stop``` for Libhoudini to install.

4. Restart Waydroid Container:
```
sudo systemctl start waydroid-container.service
```

5. Launch Waydroid:
```
waydroid show-full-ui
```

## I want to stay on X11 right now

1. Install Weston Compositor:
```
sudo apt install weston
```

2. Launch Weston:
```
weston
```

Click the terminal icon in the top left region inside the Weston window. This opens a terminal window.

3. Launch Waydroid inside Weston
```
waydroid show-full-ui
```

## Got stuck? Need help?

Feel free to ask in the Issues.
