# Windows Bluetooth HID Recognition Fix

## The Problem
Your keyboard appears in Device Manager as "compression 4c" with a yellow triangle under "Other devices" instead of under "Keyboards". This means Windows detects the Bluetooth device but doesn't recognize it as a proper HID keyboard.

## The Solution

### Step 1: Rebuild Firmware with HID Support
I've added the necessary Bluetooth HID configurations to fix this:

- `CONFIG_ZMK_HID_REPORT_TYPE_HKRO=y` - Proper HID report format
- `CONFIG_BT_GATT_HIDS=y` - Bluetooth HID service  
- `CONFIG_BT_GATT_DIS=y` - Device Information Service
- `CONFIG_BT_GATT_BAS=y` - Battery Service
- `CONFIG_BT_GATT_ENFORCE_SUBSCRIPTION=n` - Compatibility fix

**Rebuild your firmware** with these new configurations.

### Step 2: Complete Windows Cleanup

1. **Remove the current pairing:**
   - Go to Windows Settings → Bluetooth & devices
   - Find "compression 4c" and click "Remove device"

2. **Clear Device Manager entries:**
   - Open Device Manager
   - Under "Other devices", right-click "compression 4c" 
   - Select "Uninstall device"
   - Check "Delete the driver software" if available

3. **Clear keyboard Bluetooth bonds:**
   - Use the key combo: Top-left + Top-right keys (Grave + Minus)
   - Or: Space + Enter, then leftmost key

### Step 3: Fresh Pairing Process

1. **Power cycle the keyboard** - Turn it off and on
2. **Make Windows discoverable** - Settings → Bluetooth & devices → "Add device"
3. **Look for "compression 4c"** - Should appear as a keyboard this time
4. **Pair normally** - It should now install as a proper HID keyboard

## Expected Result

After following these steps:
- Device should appear under "Keyboards" in Device Manager
- Should show as "HID Keyboard Device" or "compression 4c"  
- No yellow triangle - should have proper keyboard icon
- Typing should work immediately

## If Still Not Working

### Alternative Windows Fix:
1. **Force driver installation:**
   - Device Manager → "compression 4c" (with yellow triangle)
   - Right-click → "Update driver"
   - "Browse my computer for drivers"
   - "Let me pick from a list"
   - Select "Human Interface Devices" → "HID-compliant device"

2. **Manual HID driver:**
   - Same process but choose "Keyboards" → "HID Keyboard Device"

### Registry Fix (Advanced):
If Windows still won't recognize it, there might be cached bad driver info:

1. **Run as Administrator:** `cmd`
2. **Clear Bluetooth cache:**
   ```
   net stop bthserv
   del /f /q "%systemdrive%\Windows\System32\config\systemprofile\AppData\Local\Microsoft\Windows\Bluetooth\*.*"
   net start bthserv
   ```

3. **Restart computer** and try pairing again

## Why This Happens

The original configuration was missing proper Bluetooth HID service advertisements. Windows needs specific GATT services (HID, DIS, BAS) to recognize a device as a keyboard rather than a generic Bluetooth device.

The new firmware configuration explicitly enables these services so Windows will properly identify the keyboard during the pairing process.

## Testing

After rebuilding and pairing:
1. **Check Device Manager** - Should be under "Keyboards" 
2. **Test typing** - Should work in any text field
3. **Check battery** - Should show keyboard battery level in Windows