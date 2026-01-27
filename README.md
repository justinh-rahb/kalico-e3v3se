# Kalico for Ender 3 V3 SE with Display Support

A fork of [Kalico](https://github.com/KalicoCrew/kalico) with integrated support for the stock Creality Ender 3 V3 SE display. This enables full use of the original touchscreen without any hardware modifications.

## Features

This fork combines the advanced features of Kalico (a community-maintained Klipper fork) with native display support for the Ender 3 V3 SE:

**Display Features:**
- File selection and print control from the stock display
- Print tuning (Z-offset, flow rate, speed)
- Pause/Resume/Stop controls
- Axis movement and homing
- Temperature management (nozzle and bed)
- Preheat profiles (PLA, TPU, custom)
- Motor disable
- Manual probe/leveling
- Custom macro buttons

**Multi-language Support:**
- Chinese, English, German, Russian, French, Turkish, Spanish, Italian, Portuguese, Japanese

## Requirements

- Ender 3 V3 SE with display firmware version 1.0.6
- Raspberry Pi or similar SBC running Klipper host software
- Moonraker and a web interface (Mainsail, Fluidd, etc.)

> **Note:** Creality firmware updates may alter display asset memory locations, which could affect compatibility with newer display firmware versions.

## Installation

### Building the Firmware

1. Clone this repository:
   ```bash
   git clone https://github.com/justinh-rahb/kalico-e3v3se.git ~/klipper
   ```

2. Configure the build:
   ```bash
   cd ~/klipper
   make menuconfig
   ```

   Select the following options:
   - Micro-controller Architecture: **STMicroelectronics STM32**
   - Processor model: **STM32F103**
   - Bootloader offset: **28KiB bootloader**
   - Communication interface: **Serial (on USART1 PA10/PA9)**
   - Enable extra low-level configuration: **Enable**
   - Serial bridge for USART2: **Enable**

3. Build the firmware:
   ```bash
   make
   ```

4. Copy `out/klipper.bin` to an SD card, renaming it to something unique (the printer won't flash if the filename matches the previous flash)

5. Insert the SD card into your printer and power cycle to flash

### Installing Klipper Host Software

If you haven't installed Klipper before, use [KIAUH](https://github.com/dw-0/kiauh) to install Klipper, Moonraker, and your preferred web interface.

To point KIAUH to this repository:

1. Configure KIAUH:
   ```bash
   cd ~/kiauh
   cp default.kiauh.cfg kiauh.cfg
   nano kiauh.cfg
   ```

2. Add the repository URL to your config:
   ```
   https://github.com/justinh-rahb/kalico-e3v3se, main
   ```

3. Use KIAUH's settings menu to switch Klipper source repository

### Updating Python Environment

If needed, update the Python environment:
```bash
~/klippy-env/bin/pip install -r ~/klipper/scripts/klippy-requirements.txt
```

## Configuration

Add the following section to your `printer.cfg`:

```ini
[e3v3se_display]
language: english
#logging: False
```

### Configuration Options

| Option | Default | Description |
|--------|---------|-------------|
| `language` | `english` | Display language (chinese, english, german, russian, french, turkish, spanish, italian, portuguese, japanese) |
| `logging` | `False` | Enable debug logging for the display module |

### Custom Macro Buttons

You can add custom buttons to the display's Misc menu by creating sub-sections:

```ini
[e3v3se_display macro1]
label: Calibrate
icon: 1
gcode: G28

[e3v3se_display macro2]
label: Level Bed
icon: 2
gcode: BED_MESH_CALIBRATE

[e3v3se_display macro3]
label: PID Tune
gcode: PID_CALIBRATE HEATER=extruder TARGET=200
```

#### Macro Options

| Option | Required | Description |
|--------|----------|-------------|
| `label` | No | Display text for the button (defaults to section name) |
| `icon` | No | Icon identifier for the button |
| `gcode` | Yes | G-code command(s) to execute |

## Supported Operations

| Feature | Status |
|---------|--------|
| File selection & printing | Supported |
| Print tuning | Supported |
| Pause/Resume/Stop | Supported |
| Axis movement & homing | Supported |
| Z-offset calibration | Supported |
| Temperature control | Supported |
| Preheat profiles | Supported |
| Motor disable | Supported |
| Manual probe | Supported |
| Custom macros | Supported |
| Speed adjustment | Not implemented |
| Acceleration adjustment | Not implemented |
| Steps/mm calibration | Not implemented |

## Upstream Projects

This project builds upon the work of several upstream projects:

- **[Kalico](https://github.com/KalicoCrew/kalico)** - Community-maintained Klipper fork with advanced features
- **[jpcurti/ender3-v3-se-klipper-with-display](https://github.com/jpcurti/ender3-v3-se-klipper-with-display)** - Original E3V3SE display implementation
- **[Klipper](https://github.com/Klipper3d/klipper)** - The original 3D printer firmware

## Kalico Features

This fork includes all Kalico features from the main branch. See the [Kalico documentation](https://docs.kalico.gg) for details on:

- MPC temperature control
- Velocity PID and PID profiles
- Dockable probe support
- Z calibration
- Independent X/Y acceleration and velocity
- Gcode shell commands
- And many more advanced features

## Troubleshooting

**Display not responding:**
- Verify the firmware was flashed correctly (try a different filename)
- Check that `[e3v3se_display]` is in your printer.cfg
- Enable `logging: True` to debug serial communication

**Wrong language displayed:**
- Ensure the `language` option matches one of the supported languages exactly (lowercase)

**Custom macros not appearing:**
- Verify the `gcode` option is set in each macro section
- Check Klipper logs for any configuration errors

## License

This project is licensed under the GPL-3.0 License. See the [LICENSE](COPYING) file for details.
