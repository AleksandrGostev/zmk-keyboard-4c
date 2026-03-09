# ZMK Keyboard Module: 4c

A ZMK module for the **compression 4c** keyboard - a 60-key split ergonomic keyboard with OLED display support.

## Features

- **Split design** with left and right halves
- **60 keys total** (6×5 layout per side + thumb clusters)  
- **OLED display** support (nice!view compatible)
- **Pro Micro compatible** controllers (tested with nice!nano v2)
- **Bluetooth connectivity** with enhanced TX power
- **4-layer keymap** with Base, Raise, Lower, and Bluetooth layers
- **Combos and macros** included

## Supported Hardware

- **Controllers**: Pro Micro compatible (nice!nano v2 recommended)
- **Displays**: nice!view OLED displays
- **Layout**: 6×5 split keyboard with thumb clusters

## Installation

### Using with ZMK Config Repository

1. Add this module to your `config/west.yml`:

```yaml
manifest:
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
    - name: 4c-module
      url-base: https://github.com/compressionKeyboards
  projects:
    - name: zmk
      remote: zmkfirmware
      revision: main
      import: app/west.yml
    - name: zmk-keyboard-4c
      remote: 4c-module
      revision: main
  self:
    path: config
```

2. Update your `config/build.yaml`:

```yaml
---
include:
  - board: nice_nano_v2
    shield: 4c_left nice_view
  - board: nice_nano_v2
    shield: 4c_right nice_view
```

3. Build and flash:

```bash
# Update dependencies
west update

# Build firmware
west build -p -d build/left -b nice_nano_v2 -- -DSHIELD="4c_left nice_view"
west build -p -d build/right -b nice_nano_v2 -- -DSHIELD="4c_right nice_view"
```

## Keymap

The default keymap includes 4 layers:

- **Base**: QWERTY layout with standard alphanumerics
- **Raise**: Function keys, symbols, and navigation
- **Lower**: Additional symbols and brackets  
- **Bluetooth**: Bluetooth device management

### Layer Switching

- **Raise layer**: Hold key at position [4,7] (right thumb)
- **Lower layer**: Hold key at position [4,2] (left thumb) 
- **Bluetooth layer**: Combo of positions 56+57

### Macros

- **Code fence**: Triple backticks for markdown
- **Alt+F4**: Window close shortcut

## Customization

To customize the keymap:

1. Copy the shield definition to your config:
   ```
   config/boards/shields/4c/4c.keymap
   ```

2. Modify the keymap as needed

3. The keymap will override the module's default

## Hardware Configuration

The keyboard uses these pin assignments:

### Rows (shared)
- Row 0: Pin 4
- Row 1: Pin 5  
- Row 2: Pin 6
- Row 3: Pin 7
- Row 4: Pin 3

### Columns
**Left Half:**
- Col 0: Pin 20
- Col 1: Pin 19
- Col 2: Pin 18
- Col 3: Pin 15
- Col 4: Pin 14
- Col 5: Pin 16

**Right Half:**
- Col 0: Pin 16
- Col 1: Pin 14
- Col 2: Pin 15
- Col 3: Pin 18
- Col 4: Pin 19
- Col 5: Pin 20

### Display (nice!view)
- SCK: Pin 17
- MOSI: Pin 8
- CS: Pin 0

## Configuration Options

The module includes these default configurations:

- `CONFIG_ZMK_DISPLAY=y` - Enable OLED display
- `CONFIG_ZMK_SLEEP=y` - Enable sleep mode
- `CONFIG_ZMK_KSCAN_DEBOUNCE_PRESS_MS=1` - Fast press debounce
- `CONFIG_ZMK_KSCAN_DEBOUNCE_RELEASE_MS=5` - Release debounce  
- `CONFIG_BT_CTLR_TX_PWR_PLUS_8=y` - Enhanced Bluetooth range

## License

MIT License - see LICENSE file for details.

## Support

For issues and questions:
- [ZMK Discord](https://zmk.dev/community/discord/invite)
- [ZMK Documentation](https://zmk.dev/docs/)
- [Original Repository](https://github.com/compressionKeyboards/zmk)

## Building Locally

For local development:

```bash
west build -p -d build/right -b nice_nano_v2 -- -DSHIELD="4c_right nice_view"
west build -p -d build/left -b nice_nano_v2 -- -DSHIELD="4c_left nice_view"
```

The generated `.uf2` files can be copied to your controllers for flashing.