# nice!nano v2 Discoverability Troubleshooting

If your nice!nano v2 is not appearing in Bluetooth discovery, try these steps in order:

## 1. Hardware Reset Procedure

Before testing software fixes, ensure proper hardware state:

1. **Power cycle**: Disconnect battery and USB, wait 10 seconds, reconnect
2. **Double-tap reset**: Press reset button twice quickly to enter bootloader
3. **Flash fresh firmware**: Re-flash both halves with the updated configuration

## 2. Test with Mobile Device First

Before troubleshooting Windows-specific issues:

1. Try pairing with your smartphone or tablet
2. This isolates hardware vs. Windows compatibility issues
3. If mobile pairing works, the issue is Windows-specific

## 3. Updated Configuration Changes

The latest `4c.conf` includes several fixes:

- **Windows Intel/Realtek fix**: `CONFIG_BT_CTLR_PHY_2M=n`
- **Passkey support**: `CONFIG_ZMK_BLE_PASSKEY_ENTRY=y` 
- **Enhanced advertising**: Extended advertising for better discoverability
- **Peripheral preferences**: Optimized connection parameters
- **Debug logging**: Temporarily enabled for troubleshooting

## 4. Windows-Specific Steps

If the keyboard pairs with mobile but not Windows:

### Clear Windows Bluetooth Cache
```cmd
# Run as Administrator in Command Prompt
net stop bthserv
reg delete HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\BTHPORT\Parameters\Keys /f
net start bthserv
```

### Reset Bluetooth Stack
1. Open Device Manager
2. View → Show hidden devices
3. Expand "Bluetooth" section
4. Delete all hidden/grayed devices
5. Restart computer
6. Try pairing again

### Alternative Windows Reset
1. Settings → Bluetooth & devices
2. More Bluetooth settings
3. Options tab → Reset to defaults
4. Restart Bluetooth service

## 5. ZMK Profile Reset

Clear stored pairing data:

1. **Access reset key combo**: Use the keymap's `&bt BT_CLR` binding
2. **Or use settings reset**: Flash settings_reset firmware, then normal firmware
3. **Power cycle after reset**: Restart keyboard after clearing bonds

## 6. Bootloader Issues

If none of the above works, the bootloader might need refreshing:

### Check Bootloader Version
1. Double-tap reset to enter bootloader
2. Check if `NICENANO` drive appears
3. Look for `INFO_UF2.TXT` - should show bootloader v0.6.0+

### Re-flash Bootloader (Advanced)
Only if bootloader is corrupted:
1. Download nice!nano bootloader from nice!keyboards.com
2. Use `adafruit-nrfutil` to flash bootloader
3. See [nice!nano troubleshooting docs](https://nicekeyboards.com/docs/nice-nano/troubleshooting)

## 7. Debug Information

With debug logging enabled, check for errors:

1. Connect to USB while testing Bluetooth
2. Use serial monitor to view debug output
3. Look for BLE advertising errors or initialization failures
4. Report specific error messages if found

## 8. Hardware Considerations

### Power Issues
- Ensure battery is charged (>3.7V)
- Test with USB power only
- Check for loose battery connections

### Interference
- Test away from other Bluetooth devices
- Avoid 2.4GHz WiFi interference
- Try different physical locations

## 9. Fallback Options

If discoverability still fails:

### USB Mode (Temporary)
Create a USB-only configuration for testing:
```
CONFIG_ZMK_USB=y
CONFIG_ZMK_BLE=n
```

### Single Half Test
Test one half at a time to isolate split-specific issues

## 10. Getting Help

If all steps fail:
1. Document exact steps attempted
2. Include debug log output
3. Note mobile device pairing results
4. Report to ZMK community with full details

---

**Note**: The updated configuration includes Windows compatibility fixes that resolve most discoverability issues. If problems persist after following these steps, there may be a hardware-specific issue requiring further investigation.