# ESP32-S3-Touch-LCD-1.46B PlatformIO Project

This repository provides a template and guide for using the [Waveshare ESP32-S3-Touch-LCD-1.46B](https://www.waveshare.com/wiki/ESP32-S3-Touch-LCD-1.46B) with **PlatformIO**. 

Official documentation from Waveshare primarily covers ESP-IDF and the Arduino IDE. This project bridges the gap for users who prefer the advanced features of PlatformIO.

## Getting Started

To use this project, you need to set up a custom board definition and configure your partitions correctly to support the 16MB Flash and PSRAM on this board.

### 1. Migrating from Arduino IDE

If you are coming from the Waveshare Wiki examples:

1.  **Copy Source Files**: Copy all `.cpp`, `.h`, and `.ino` files from the Waveshare Arduino example into the `src/` directory of your PlatformIO project.
2.  **Convert `.ino` to `.cpp`**: Rename your primary sketchbook file (e.g., `Example.ino`) to `main.cpp`.
3.  **Include Arduino Header**: Ensure `main.cpp` starts with `#include <Arduino.h>` (if not already present).
4.  **Fix Function Prototypes**: Unlike the Arduino IDE, PlatformIO requires function prototypes to be declared before they are used, or defined before their first call.

### 2. Custom Board Definition

Create a `boards` directory in your project root and add [waveshare_esp32_s3_lcd_146.json](boards/waveshare_esp32_s3_lcd_146.json). 

This file defines the hardware capabilities:
- **MCU**: ESP32-S3
- **Flash**: 16MB (QIO)
- **PSRAM**: 8MB (OPI)
- **Core**: Arduino/ESP-IDF

### 3. Partition Table

The ESP32-S3 on this board has 16MB of Flash. Standard partition schemes may not utilize the full space or might conflict with PSRAM settings.

Create a `partitions` directory and add your CSV file (e.g., [esp_sr_16.csv](partitions/esp_sr_16.csv)). This allows you to allocate space for:
- App partitions (OTA)
- SPIFFS for assets
- Specialized models (like ESP-SR for voice recognition)

### 4. PlatformIO Configuration

Your `platformio.ini` should look like this:

```ini
[env:waveshare_esp32_s3_lcd_146]
platform = espressif32
board = waveshare_esp32_s3_lcd_146
framework = arduino

monitor_speed = 115200
upload_speed = 921600

board_build.psram = enabled
; Point to your custom partition file
board_build.partitions = partitions/esp_sr_16.csv

build_flags = 
    -DBOARD_HAS_PSRAM
```

## Features
- **Display**: 1.46" Circular Touch LCD (SPD2010 Driver)
- **Audio**: PCM5101 DAC
- **Sensors**: QMI8658 Gyroscope/Accelerometer
- **Storage**: MicroSD Card Slot
- **Connectivity**: WiFi & Bluetooth 5.0

## Troubleshooting

- **Blank Screen**: Ensure `board_build.psram = enabled` is set and `BOARD_HAS_PSRAM` flag is defined. The display driver often relies on PSRAM for the frame buffer.
- **Upload Failures**: Check that the `upload_speed` is compatible with your USB-to-Serial converter.

## Credits & Resources
- [Waveshare Wiki](https://www.waveshare.com/wiki/ESP32-S3-Touch-LCD-1.46B)
- [LVGL Documentation](https://docs.lvgl.io/)
