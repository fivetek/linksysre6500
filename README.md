# Linksys RE6500
Linksys RE6500 AC1200 is a Dual-Band Wireless Range Extender and Access Point. Flashing the default firmware of the Linksys RE6500 AP with Gargoyle and transform the the Linksys RE6500 Access Point in a Router.

*### The official firmware does checks on the image and rejects non-linksys images so it can't be used to flash Gargoyle. So the method used is thru UART and TFTP ###*

# Flashing Gargoyle Firmware on Linksys RE6500 (Access Point)

Gargoyle is a free OpenWrt-based Linux distribution for a range of wireless routers based on Broadcom, Atheros, MediaTek and others chipsets, Asus Routers, Netgear, Linksys and TP-Link routers.
Official Website: https://www.gargoyle-router.com/

This guide explains how to install [Gargoyle Router Firmware](https://www.gargoyle-router.com/) on the **Linksys RE6500** access point.  
Gargoyle is based on [OpenWrt](https://openwrt.org/), with a user-friendly web interface and advanced features like bandwidth monitoring, quotas, and QoS.

---

## ⚠️ Disclaimer
- Flashing third-party firmware **voids your warranty**.  
- There is always a risk of **bricking your device** if something goes wrong.  
- Follow this guide at your own risk.  

---

## 📦 Requirements

- **Linksys RE6500** access point
- **Gargoyle firmware image** for RE6500  
  (Download from [Gargoyle official site](https://www.gargoyle-router.com/download.php))
- **PC with TFTP server** installed  
  (Linux: `tftpd-hpa`, Windows: `tftpd32/64`, macOS: `tftp`)
- **USB-to-UART adapter** (3.3V TTL) + jumper wires  
- Serial terminal software (e.g. `minicom`, `screen`, `putty`)  
- Ethernet cable  

---

## 🔌 Step 1: Connect to RE6500 via UART and to a TFTP

1. Open the RE6500 case and locate the **UART header**.  
   - TX, RX, GND pins (3.3V logic).  
2. Connect your USB-UART adapter:  
   - **GND → GND (Pin )**
   - **TX → RX ()**  
   - **RX → TX ()**  
   - ⚠️ **Do not connect VCC (3.3V) (Pin 4)**.
- Board Header pins (The header pins don't come soldered to the board you have to solder them by yourself)

![alt text](https://github.com/fivetek/linksysre6500/blob/main/img/board_headerpins.jpeg)

3. Start serial terminal at 57600 baud, 8N1. Set the environment variables in serial console.

  `screen /dev/ttyUSB0 57600`

  Boot without reset held down. Watch the console for U-Boot, and in particular, this line: *relocate_code Pointer at: 83f20000*

  Now press and hold reset, and type *1234567890 RET 4 RET*

  Set the environment variables in the serial terminal:

  `setenv ipaddr 192.168.1.1`

  `setenv serverip 192.168.1.2`

---

## 🌐 Step 2: Configure TFTP Server

1. Set your PC IP to `192.168.1.2`.  
2. Place the Gargoyle **initramfs image** in your TFTP root directory.
