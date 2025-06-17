# AIC8800 Linux Driver AX900 Wifi Adapter Linux

This project provides a Linux driver for the AIC8800 chipset, supporting both USB and SDIO interfaces.

## Table of Contents
- [Project Overview](#project-overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
  - [Compiling the Driver](#compiling-the-driver)
  - [Installing the Driver](#installing-the-driver)
- [Uninstalling other Drivers that you might have tried and now showing up when running sudo apt update]
- [Usage](#usage)
- [License](#license)

## Project Overview

The AIC8800 Linux Driver supports the AIC8800 chipset for wireless communication, enabling functionality on devices using Linux-based operating systems. This driver is compatible with various kernel versions and can be used with different hardware configurations, such as USB or SDIO interfaces.

## Features

- Support for USB interface
- FullMAC driver with 802.11ac capabilities
- WPA/WPA2 encryption
- MAC randomization support
- Power management features (DCDC_VRF mode)
- WPA3 compatibility (for kernels supporting it)
- MU-MIMO support (requires compatible firmware)
- Wireless extensions support

## Requirements

To compile and install this driver, ensure the following dependencies are installed:

- Linux kernel headers and development files - you need to install the kernel headers. (google how to do that)
- GCC and Make
- Git (for cloning the repository)

```bash
# Fedora example
sudo dnf install kernel-devel kernel-headers gcc make git
```

```bash
# Debian
sudo apt update
sudo apt install linux-headers-$(uname -r) build-essential git
```
## Installation

### Compiling the Driver

1. Clone the repository:

   ```bash
   git clone https://github.com/davidreact/aic8800_linux_drvier.git
   cd aic8800_linux_drvier
   ```

2. Compile the driver:

   ```bash
   make
   ```

   This will generate the necessary kernel module (`.ko` file).

### Installing the Driver

3. Install the compiled driver:

   ```bash
   sudo make install
   ```

4. Load the driver:

   ```bash
   sudo modprobe aic8800_fdrv
   ```

5. To verify the driver is loaded, run:

   ```bash
   lsmod | grep aic8800_fdrv
   ```

### Uninstalling the Driver

If you need to remove the driver:

```bash
sudo make uninstall
```

### Uninstalling other Drivers that you might have tried and now showing up when running sudo apt update

```bash
sudo apt-get remove <package_name>
```
I had to uninstall two packages that were erroring which removed the wifi adapter again. Go back to the folder on step 2 and run step 3 and 4.

## Usage

Once the driver is installed and loaded, the AIC8800 chipset will be automatically recognized by the Linux system. You can verify the wireless device is working by checking the network interfaces:

```bash
ip link
```

You can also manage the wireless device using standard Linux network management tools like `iwconfig`, `ifconfig`, or `nmcli`.

## Change the device from USB to WIFI (not sure if you need to do this, test it with the above but if it is still not showing up then see below)

First find out the vendor (-v) and product id (-p) of your modem. use the code below and plug the wifi modem.

```bash
sudo dmesg -HW
```
It should look something like this

![image](https://github.com/user-attachments/assets/3d109db9-33a6-451f-aac0-b3dfa9dca42b)

Using the above details you can change the paremeters below. You could also try lsusb as a way to see the connected devices.

```bash
sudo /usr/sbin/usb_modeswitch -KQ -v a69c -p 5723
```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

