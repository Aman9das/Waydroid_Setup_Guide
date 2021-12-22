# Waydroid Guide

This guide takes you through the process of installing waydroid, and getting arm translation on Linux.

[COMPATIBILITY.md](COMPATIBILITY.md) is the table of app compatibility. You're encouraged to request for your favourite apps' compatibility status on the issues page.

<details>
<summary>
<h3>System details:<h3>
</summary>

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
</details>

<details>
<summary>
<h2>Pre-requisites:</h2>
</summary>

1. Follow the pre-requisites section: https://docs.waydro.id/usage/install-on-desktops#install-pre-requisites

2. Download lineageOS system images archive from SourceForge: https://sourceforge.net/projects/blissos-dev/files/waydroid/lineage/lineage-18.1/

(Use a download manager like [fireDM](https://github.com/firedm/FireDM) in case of slow download speeds)

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
sudo apt install git libglib2.0-dev libglibutil-dev gcc
git clone --depth=1 https://github.com/mer-hybris/libgbinder
cd libgbinder
make
make install
```

7. Copy the new ```libgbinder.so.1```, ```libgbinder.so.1.1``` and ```libgbinder.so.1.1.14``` files added to /usr/lib to wherever your system has the current libgbinder installed. Use ```dpkg -L libgbinder``` to find out.

</details>
<details>
<summary><h2>Install Waydroid</h2></summary>

1. Follow the install waydroid section : https://docs.waydro.id/usage/install-on-desktops#install-waydroid

If confused with the "bullseye" replacement, check ```VERSION_CODENAME``` in the output of:
```
cat /etc/os-release
```

```impish``` users need not replace ```bullseye```

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
You may need to sign ashmem_linux manually for secure boot. <details><summary>error details</summary>```modprobe: ERROR: could not insert 'ashmem_linux': Operation not permitted```</details>
```
sudo update-secureboot-policy --new-key
sudo /usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 /var/lib/shim-signed/mok/MOK.priv /var/lib/shim-signed/mok/MOK.der $(modinfo -n ashmem_linux)
```

</details>
<details>
<summary>
<h2>Arm Translation</h2>
</summary>

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
</details>
<details>
<summary>
<h2>I want to stay on X11 right now</h2>
</summary>

Most beginner friendly distros besides Linux Mint Cinnamon do have Wayland pre-installed. Weston can leverage this wayland backend and run Waydroid.

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

You may need to ```sudo waydroid container restart``` to restart the android image before launching inside weston.
</details>
<br>

## Bibliography

This guide has been made possible thanks to these original sources and projects.

1. Jon West: https://t.me/WayDroid/55770 (Major Source)
2. Waydroid: https://waydro.id/
3. Waydroid Script: https://github.com/casualsnek/waydroid_script/
4. BlissOS: https://blissos.org/

## [Want to Contribute?](CONTRIBUTING.md)

## Need help?

Odds are something slipped through at our end.

Creating a bug report, and improving the guide helps the next person who follows the guide.

The good folks at the Waydroid [matrix](https://matrix.to/#/#waydroid:connolly.tech) and [telegram](https://t.me/WayDroid) chatrooms are very helpful too, in case you need advanced troubleshooting.
