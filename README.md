# ZMK Keyboard Module: 4c

A ZMK module for the **compression 4c** keyboard - a 60-key split ergonomic keyboard.

## Features

- **Split design** with left and right halves
- **60 keys total** (6×5 layout per side + thumb clusters)  
- **Pro Micro compatible** controllers (tested with nice!nano v2)
- **Bluetooth connectivity** with enhanced TX power (Bluetooth-only by default)
- **4-layer keymap** with Base, Raise, Lower, and Bluetooth layers
- **Combos and macros** included
- **OLED display** support (nice!view compatible, enabled by default)
- **Optional USB support** (can be enabled if needed)

## Supported Hardware

- **Controllers**: Pro Micro compatible (nice!nano v2 recommended)
- **Displays**: nice!view OLED displays (optional)
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

**With nice!view displays (default):**
```yaml
---
include:
  - board: nice_nano_v2
    shield: 4c_left nice_view
  - board: nice_nano_v2
    shield: 4c_right nice_view
```

**Without displays:**
```yaml
---
include:
  - board: nice_nano_v2
    shield: 4c_left
  - board: nice_nano_v2
    shield: 4c_right
```

3. Build and flash:

```bash
# Update dependencies
west update

# Build firmware (without display)
west build -p -d build/left -b nice_nano_v2 -- -DSHIELD="4c_left"
west build -p -d build/right -b nice_nano_v2 -- -DSHIELD="4c_right"

# Build firmware (with display)
west build -p -d build/left -b nice_nano_v2 -- -DSHIELD="4c_left nice_view"
west build -p -d build/right -b nice_nano_v2 -- -DSHIELD="4c_right nice_view"
```

## Enable Display Support (Optional)

To enable nice!view display support, you have two options:

**Option 1: Modify the shield config files**
1. In your ZMK config, create override files:
   - `config/boards/shields/4c/4c.conf`
   - `config/boards/shields/4c/4c_left.conf` 
   - `config/boards/shields/4c/4c_right.conf`

2. In each file, change:
   ```
   CONFIG_ZMK_DISPLAY=n
   ```
   to:
   ```
   CONFIG_ZMK_DISPLAY=y
   ```

3. Build with the nice_view shield:
   ```bash
   west build -d build/left -b nice_nano_v2 -- -DSHIELD="4c_left nice_view"
   west build -d build/right -b nice_nano_v2 -- -DSHIELD="4c_right nice_view"
   ```

**Option 2: Use build-time configuration**
1. Add to your keymap config file:
   ```
   CONFIG_ZMK_DISPLAY=y
   ```

2. Build with nice_view shield as above.

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

## Configuration Options

The module includes these default configurations:

- `CONFIG_ZMK_DISPLAY=y` - OLED display enabled (nice!view support)
- `CONFIG_ZMK_USB=n` - USB HID disabled (Bluetooth-only by default)
- `CONFIG_ZMK_SLEEP=y` - Enable sleep mode
- `CONFIG_ZMK_KSCAN_DEBOUNCE_PRESS_MS=1` - Fast press debounce
- `CONFIG_ZMK_KSCAN_DEBOUNCE_RELEASE_MS=5` - Release debounce  
- `CONFIG_BT_CTLR_TX_PWR_PLUS_8=y` - Enhanced Bluetooth range

## Disable Display Support (If Needed)

If you want to build without display support, override the configuration in your ZMK config repository:

1. **Create config override files:**
   - `config/boards/shields/4c/4c.conf`
   - `config/boards/shields/4c/4c_left.conf` 
   - `config/boards/shields/4c/4c_right.conf`

2. **In each file, add:**
   ```
   CONFIG_ZMK_DISPLAY=n
   ```

3. **Build without nice_view shield:**
   ```bash
   west build -d build/left -b nice_nano_v2 -- -DSHIELD="4c_left"
   west build -d build/right -b nice_nano_v2 -- -DSHIELD="4c_right"
   ```

## Enable USB Support (Optional)

This keyboard is configured as Bluetooth-only by default. To enable USB support:

1. In your ZMK config, override the configuration:
   ```
   CONFIG_ZMK_USB=y
   ```

2. This will enable USB HID functionality for wired usage.

Note: The right half acts as the central device and handles USB connectivity when enabled.

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
# Without display
west build -p -d build/right -b nice_nano_v2 -- -DSHIELD="4c_right"
west build -p -d build/left -b nice_nano_v2 -- -DSHIELD="4c_left"

# With display  
west build -p -d build/right -b nice_nano_v2 -- -DSHIELD="4c_right nice_view"
west build -p -d build/left -b nice_nano_v2 -- -DSHIELD="4c_left nice_view"
```

The generated `.uf2` files can be copied to your controllers for flashing.