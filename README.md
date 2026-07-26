# ImmortalWrt 25.12.1 Firmware for SDMC NR3053 (256MB NAND Flash)

This is a personal Firmware and APK/OPKG Repository dedicated to the **SDMC NR3053** router (Powered by MediaTek MT7981 SoC, 512MB RAM, 256MB SPI NAND Flash). 

This custom firmware includes hardware optimizations, fixes the `fit0` rootfs bootloop issue, fully recognizes the 256MB NAND Flash, and comes pre-configured with a 100% kernel-hash-matched package repository.

---

## ⚠️ DISCLAIMER ⚠️
- These instructions are **ONLY INTENDED FOR ROUTERS THAT ARE ALREADY UNLOCKED AND RUNNING OPENWRT / CUSTOM U-BOOT**.
- Overwriting bootloader partitions (BL2 and FIP) is an **extremely dangerous operation**. Any mistake (flashing to wrong partitions, power interruption) WILL permanently hard-brick your router, requiring chip desoldering and a CH341A hardware programmer to recover.
- **I take no responsibility** for any damages, data loss, or bricked devices resulting from the use of files or instructions in this repository. Proceed strictly at your own risk!

---

## 🛠 INSTALLATION GUIDE

### STEP 1: Flash Standard NR3053 U-Boot (Mandatory)
To ensure proper partition alignment and system stability, you must overwrite your current U-Boot with the standard NR3053 U-Boot provided in this repository.

1. Download `mt7981-sdmc-nr3053-bl2.bin` and `mt7981-sdmc-nr3053-fip.bin` from this repository to your computer.
2. Upload both files to the `/tmp/` directory on your router using WinSCP or MobaXterm (SFTP).
3. SSH into your router (using PuTTY or MobaXterm).
4. Verify your current partition table layout:
   ```bash
   cat /proc/mtd
   ```
   *(Ensure that `bl2` and `fip` partition names exist in the output).*
5. Flash both bootloader partitions (DOUBLE-CHECK PARTITION NAMES BEFORE EXECUTING):
   ```bash
   mtd write /tmp/mt7981-sdmc-nr3053-bl2.bin bl2
   mtd write /tmp/mt7981-sdmc-nr3053-fip.bin fip
   ```

### STEP 2: Flash ImmortalWrt 25.12.1 Firmware
After flashing U-Boot, choose one of the following methods to install the firmware:

**Method 1: Flash directly via SSH (Recommended)**
Still in the SSH session, upload the Sysupgrade image (from the Releases section) to `/tmp/` and run:
```bash
sysupgrade -n /tmp/immortalwrt-mediatek-filogic-sdmc_nr3053-squashfs-sysupgrade.bin
```

**Method 2: Flash via Recovery Mode (TFTP / UART)**
If the router fails to boot into the operating system, access the U-Boot recovery environment:
1. Use TFTP to boot `immortalwrt-mediatek-filogic-sdmc_nr3053-initramfs-recovery.itb` directly into RAM.
2. Log into the temporary LuCI web interface at `192.168.1.1`.
3. Go to **System -> Backup / Flash Firmware** -> Upload `immortalwrt-mediatek-filogic-sdmc_nr3053-squashfs-sysupgrade.bin`.
4. **UNCHECK "Keep settings"** and proceed to flash.

---

## 📦 INTEGRATED PACKAGE REPOSITORY
The firmware images in the Releases section come pre-configured to automatically point to this GitHub Pages repository. When you navigate to LuCI and click `Update lists`, the router will seamlessly fetch `kmod` packages that match your kernel build with zero signature or vermagic mismatch errors.

Good luck!
