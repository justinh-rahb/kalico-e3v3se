# Ender-3 V3 SE Klipper (Kalico Port) Installation Guide

This repository contains a port of the Ender-3 V3 SE display changes to the Kalico architecture.

## 1. Repository Location
The ported code is available on GitHub:
**https://github.com/justinh-rahb/kalico-e3v3se**

Clone it to your home directory or preferred location:
```bash
git clone https://github.com/justinh-rahb/kalico-e3v3se.git ~/klipper-e3v3se
```

## 2. Build Firmware

1. **Clean and Configure**
   ```bash
   cd ~/klipper-e3v3se
   make clean
   make menuconfig
   ```

2. **Menuconfig Settings**
   * **Micro-controller Architecture**: `STMicroelectronics STM32`
   * **Processor model**: `STM32F103` (Confirm this matches your specific board chip if different)
   * **Bootloader offset**: `28KiB bootloader`
   * **Communication interface**: `Serial (on USART1 PA10/PA9)`
   * **Serial Bridge**: `[*] Enable Serial Bridge` (NEW SETTING)
     * **Bridge USART**: `USART2`

   *Note: If your board uses the GD32F303 chip, it is usually compatible with the STM32F103 setting.*

3. **Compile**
   ```bash
   make
   ```

## 3. Flash Firmware

1. Locate the file `out/klipper.bin`.
2. Rename it to something unique, e.g., `klipper_kalico_v1.bin`.
3. Copy it to the root of your SD card.
4. Insert SD card into printer and power cycle.
5. Wait ~10 seconds. The screen might stay blank or show text depending on the flash status.

## 4. Host Configuration (Klipper Service)

To use this new code version instead of your existing Klipper installation:

1. **Stop Klipper**
   ```bash
   sudo systemctl stop klipper
   ```

2. **Switch Klipper Directory**
   (Assuming your current klipper is at `~/klipper`)
   ```bash
   mv ~/klipper ~/klipper.bak
   ln -s ~/klipper-e3v3se ~/klipper
   ```
   *Alternatively, update your systemd service file to point to the new directory.*

3. **Update printer.cfg**
   Add the following section to your `printer.cfg`:
   ```ini
   [e3v3se_display]
   language: english
   logging: True
   ```

4. **Restart Klipper**
   ```bash
   sudo systemctl start klipper
   ```

## 5. Verification
* Check `klippy.log` for any errors related to `e3v3se_display`.
* Verify that the display shows the interface and not the screensaver.
