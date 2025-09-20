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
   - **GND → GND (PIN 3)**
   - **TX → RX (PIN 1)**  
   - **RX → TX (PIN 2)**  
   - ⚠️ **Do not connect VCC (3.3V) (Pin 4)**.
- Board Header pins (The header pins don't come soldered to the board you have to solder them by yourself)

![alt text](https://github.com/fivetek/linksysre6500/blob/main/img/board_headerpins.jpeg)

3. Start serial terminal at 57600 baud, 8N1. Set the environment variables in serial console.

 ```bash
 screen /dev/ttyUSB0 57600
```

---

## 🌐 Step 2: Configure TFTP Server

1. Set your PC IP to `192.168.1.2`.  
2. Place the Gargoyle **initramfs image** in your TFTP root directory.

---

## 🖥️ Step 3: Boot RE6500 into U-Boot

1. Power on the RE6500, Boot without reset held down.
2. Watch the console for U-Boot, and in particular, this line: *relocate_code Pointer at: 83f20000*
3. Now press and hold reset, and type *1234567890 RET 4 RET*
4. Set the environment variables in the serial terminal:

```bash
setenv ipaddr 192.168.1.1
setenv serverip 192.168.1.2
saveenv
```

## 📥 Step 4: Load and Boot Gargoyle (RAM)

```bash
tftpboot 0x81000000 gargoyle_1.14.0-ramips-mt7621-linksys_re6500-initramfs-kernel.bin
```

```bash
bootm 0x81000000
```
The device will boot Gargoyle from RAM (not yet written to flash).

## 💾 Step 5: Flash Gargoyle Permanently

1. Access the Gargoyle web interface at: http://192.168.1.1
2. Navigate to System → Firmware Upgrade.
3. Upload the sysupgrade image.
4. Select "Do not keep settings".
5. Start upgrade and wait for the RE6500 to reboot.

## ✅ Step 6: First Login

Default IP: 192.168.1.1

Default user: root

Password: password **CHANGE AFTER FIRST LOGIN**

## 📚 Resources

[Gargoyle Official Site](https://www.gargoyle-router.com/)

[OpenWrt Wiki – Linksys RE6500](https://openwrt.org/toh/linksys/re6500)
