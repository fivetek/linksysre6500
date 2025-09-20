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

## 🔌 Step 1: Connect to RE6500 via UART

1. Open the RE6500 case and locate the **UART header**.  
   - TX, RX, GND pins (3.3V logic).  
2. Connect your USB-UART adapter:  
   - **GND → GND**  
   - **TX → RX**  
   - **RX → TX**  
   - ⚠️ **Do not connect VCC (3.3V)**.
![alt text](https://github.com/fivetek/linksysre6500/blob/main/img/board_headerpins.jpeg)

3. Start serial terminal at `57600 baud, 8N1`.

---
