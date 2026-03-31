# R2P2-ESP32 Installer

Web-based firmware installer for R2P2-ESP32 (PicoRuby on ESP32).

https://kishima.github.io/R2P2-ESP32-installer/

## Usage

1. Connect your ESP32 device via USB
2. Open the installer page in Chrome, Edge, or Opera
3. Click **Connect and Flash** and select the serial port
4. Wait for flashing to complete
5. Click **Connect to Serial** to open the serial console

## Requirements

- Browser with Web Serial API support (Chrome, Edge, Opera)
- USB-connected ESP32 device

## Updating firmware

### Getting bin files

Build R2P2-ESP32 and copy the following files into `firmware/`:

```
build/bootloader/bootloader.bin     -> firmware/bootloader.bin
build/partition_table/partition-table.bin -> firmware/partition-table.bin
build/R2P2-ESP32.bin                -> firmware/R2P2-ESP32.bin
build/storage.bin                   -> firmware/storage.bin
```

### manifest.json

`manifest.json` defines which bin files to flash and at what offsets.

```json
{
  "name": "R2P2-ESP32",
  "version": "1.0.0",
  "builds": [
    {
      "chipFamily": "ESP32",
      "parts": [
        { "path": "firmware/bootloader.bin",       "offset": 4096 },
        { "path": "firmware/partition-table.bin",   "offset": 32768 },
        { "path": "firmware/R2P2-ESP32.bin",        "offset": 65536 },
        { "path": "firmware/storage.bin",           "offset": 2162688 }
      ]
    }
  ]
}
```

Offset values are derived from:

| Part | Offset | Source |
|------|--------|--------|
| bootloader.bin | 0x1000 (4096) | ESP-IDF default |
| partition-table.bin | 0x8000 (32768) | ESP-IDF default |
| R2P2-ESP32.bin | 0x10000 (65536) | `partitions.csv` factory offset |
| storage.bin | 0x210000 (2162688) | factory offset + factory size (0x10000 + 2M) |

If `partitions.csv` changes, update the offsets accordingly.
