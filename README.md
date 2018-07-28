HW PROBE 1.4
============

Hardware Probe Tool (HW Probe) — a tool to probe for hardware, check its operability and upload result to the Linux hardware database: https://linux-hardware.org

Contents
--------

1.  [ About        ](#about)
2.  [ Install      ](#install)
3.  [ Usage        ](#usage)
4.  [ AppImage     ](#appimage)
5.  [ Docker       ](#docker)
6.  [ Live ISO     ](#live-iso)
7.  [ Inventory    ](#inventory)
8.  [ Offline view ](#offline-view)
9.  [ ACPI dump    ](#acpi-dump)
10. [ Operability  ](#operability)
11. [ Privacy      ](#privacy)


About
-----

Probe — is a snapshot of your computer's hardware state and system logs. HW Probe tool returns a permanent URL to view the probe of the computer.

Sample probe: https://linux-hardware.org/?probe=b394035f90

Share your probes and logs with Linux developers in order to debug and fix problems on your computer. Simplify inventory of hardware in your company.

You can make a probe of your computer with the help of [AppImage](#appimage), [Docker image](#docker), [Live ISO](#live-iso) or RPM/DEB package.

By creating probes you contribute to the "HDD/SSD Real-Life Reliability Test" study: https://github.com/linuxhw/SMART


Install
-------

You can probe your computer by [AppImage](#appimage), [Docker image](#docker) or [Live ISO](#live-iso).

Also you can install native RPM/DEB package for your Linux distribution or install from source. See all install instructions in the [INSTALL.md](https://github.com/linuxhw/hw-probe/blob/master/INSTALL.md) file.


Usage
-----

Make a probe:

    sudo hw-probe -all -upload -id "DESC"

DESC — any description of the probe.


AppImage
--------

The portable app that runs anywhere, no need to install anything. Just download [hw-probe-1.4-97-x86_64.AppImage](https://github.com/linuxhw/hw-probe/releases/download/1.4/hw-probe-1.4-97-x86_64.AppImage) and run the following command to probe your computer:

    chmod +x ./hw-probe-1.4-97-x86_64.AppImage
    sudo ./hw-probe-1.4-97-x86_64.AppImage -all -upload

###### Supported systems

The app runs on all Linux distributions with `Glibc >= 2.14` including:

* Ubuntu 12.04 and later
* Linux Mint 13 and later
* Debian 8 and later
* openSUSE 12.0 and later
* Manjaro 0.8 and later
* ROSA Linux R1 and later
* Fedora 15 and later (need to add `fuse-libs` package to host on Fedora 15, 16 and 17)
* CentOS 7 and later
* Alt Linux 7 and later
* Gentoo 12 and later

Docker
------

You can easily make a probe on any Linux distribution without installing the tool with the help of the Docker image:

    sudo docker run -it \
    -v /dev:/dev:ro \
    -v /lib/modules:/lib/modules:ro \
    -v /etc/os-release:/etc/os-release:ro \
    -v /var/log:/var/log:ro \
    --privileged --net=host --pid=host \
    linuxhw/hw-probe -all -upload -id DESC

You may need to run `xhost +local:` before docker run to collect X11 info (xrandr, xinput, etc.).

Docker hub repository: https://hub.docker.com/r/linuxhw/hw-probe/


Live ISO
--------

If the tool is not pre-installed in your system or you have troubles with installing the tool or its dependencies (e.g. hwinfo is not available in the repository) then try this Linux ISO with hw-probe installed: https://mirror.yandex.ru/rosa/rosa2016.1/iso/ROSA.Fresh.R10/

Boot this Linux ISO in Live mode on your computer and make a probe (see USAGE).


Inventory
---------

Request inventory ID:

    hw-probe -get-inventory-id

Mark your probes by this ID:

    sudo hw-probe -all -upload -id DESC -inventory-id ID

Find your computers by the inventory ID on this page: https://linux-hardware.org/?view=computers


Offline view
------------

Create your probes collection view for offline use:

    sudo hw-probe -import DIR


ACPI dump
---------

Dump and decode ACPI table:

    sudo hw-probe -all -upload -dump-acpi -decode-acpi

NOTE: "acpica-tools" package should be installed


Operability
-----------

The tool checks operability of devices on board by analysis of collected log files. You can perform additional operability sanity tests by the following command:

    sudo hw-probe -all -check -upload

The following tests are executed:

* graphics test by `glxgears` (for both integrated and discrete graphics cards)
* drive read speed test by `hdparm` (for all HDDs and SSDs)
* CPU performance test by `dd` and `md5sum`
* RAM memory test by `memtester`

Execution time is about 1 min for average modern desktop hardware.


Privacy
-------

Private information (including the username, machine's hostname, IP addresses, MAC addresses and serial numbers) is NOT uploaded to the database.

The tool uploads SHA512 hash of MAC addresses and serial numbers to properly identify unique computers and hard drives. All the data is uploaded securely via HTTPS.


Enjoy!
