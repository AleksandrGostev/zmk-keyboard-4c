# Windows Bluetooth HID Recognition Fix

## The Problem
Your keyboard appears in Device Manager as "compression 4c" with a yellow triangle under "Other devices" instead of under "Keyboards". This means Windows detects the Bluetooth device but doesn't recognize it as a proper HID keyboard.

## The Root Cause
ZMK keyboards sometimes have this issue where Windows can't automatically determine the device type during initial pairing. The device is working correctly, but Windows needs help recognizing it as a keyboard.

## The Solution

### Method 1: Manual Driver Installation (Recommended)

1. **Keep the device paired** - Don't unpair it from Windows
2. **Open Device Manager** - Right-click Windows button → Device Manager
3. **Find "compression 4c"** under "Other devices" (with yellow triangle)
4. **Right-click → "Update driver"**
5. **"Browse my computer for drivers"**
6. **"Let me pick from a list of available drivers"**
7. **Select "Human Interface Devices"** → Click Next
8. **Select "HID-compliant device"** or **"HID Keyboard Device"** → Click Next
9. **Install** - Windows will apply the proper driver

### Method 2: Force Keyboard Driver

If Method 1 doesn't work:

1. **Same process** but in step 7, select **"Keyboards"**
2. **Select "HID Keyboard Device"** or **"Standard PS/2 Keyboard"**
3. **Click Next** and install

### Method 3: Fresh Pairing (If above methods fail)

1. **Clear keyboard bonds:**
   - Press Top-left + Top-right keys (Grave + Minus) on keyboard
   - Or: Space + Enter, then leftmost key

2. **Remove from Windows:**
   - Settings → Bluetooth & devices → "compression 4c" → Remove

3. **Clear Device Manager:**
   - Device Manager → "compression 4c" → Uninstall device

4. **Restart computer**

5. **Pair fresh** - Sometimes Windows will recognize it correctly on second attempt

## Expected Result

After applying the driver fix:
- Device moves from "Other devices" to "Keyboards" in Device Manager
- No yellow triangle - shows proper keyboard icon
- Typing works immediately in any application
- May show battery level in Windows

## Why This Happens

This is a common Windows Bluetooth issue, not specific to ZMK. Windows sometimes can't automatically determine that a Bluetooth device is a keyboard during the initial pairing handshake. The device is functioning correctly - Windows just needs the manual driver hint.

## Alternative Solutions

### Registry Fix (Advanced Users):
If the issue persists, Windows may have cached incorrect device information:

```cmd
# Run as Administrator
net stop bthserv
del /f /q "%systemdrive%\Windows\System32\config\systemprofile\AppData\Local\Microsoft\Windows\Bluetooth\*.*"  
net start bthserv
```

### Third-party Tools:
- **Bluetooth Driver Installer** - Can force proper driver installation
- **USBDeview** - Can help remove cached device entries

## Testing

After fixing the driver:
1. **Check Device Manager** - Should appear under "Keyboards"
2. **Test typing** - Should work in Notepad, browser, etc.
3. **Check Windows Settings** - May show keyboard battery level
4. **Test function keys** - F1-F12, media keys should work

## Prevention

For future keyboards:
- Windows may remember the driver association
- Subsequent ZMK keyboards might pair correctly automatically  
- Keep this guide handy for troubleshooting other custom keyboards

The key point is that **the keyboard firmware is working correctly** - this is purely a Windows driver recognition issue that's easily fixed with manual driver selection.