# Bluetooth Troubleshooting Guide for 4c Keyboard

## Quick Fixes to Try First

### 1. **Clear Bluetooth Bonds (Most Important)**
The keyboard might have old pairing data that's preventing new connections.

**Method 1 - Key Combo:**
- Press and hold the **top-left** and **top-right** keys simultaneously (Grave + Minus)
- This triggers `&bt BT_CLR` to clear all Bluetooth bonds

**Method 2 - Bluetooth Layer:**
- Press **Space + Enter** together to access Bluetooth layer
- Press the **leftmost key** (top row) to clear bonds
- Or press the **rightmost key** (top row) to clear bonds

### 2. **Reset Procedure**
1. Clear Bluetooth bonds (using method above)
2. Power off both halves
3. Power on RIGHT half first (this is the central device)
4. Wait 10 seconds
5. Power on LEFT half
6. Try pairing again

## Detailed Troubleshooting Steps

### Step 1: Verify Which Half is Central
- **RIGHT half** = Central (connects to your computer/phone)
- **LEFT half** = Peripheral (connects to right half)
- Only flash firmware to the RIGHT half first for testing

### Step 2: Check Bluetooth Discoverability
The keyboard should appear as **"compression 4c"** in Bluetooth settings.

**If not visible:**
1. Make sure you're using the RIGHT half only for initial testing
2. Clear Bluetooth bonds using key combos above
3. Restart the controller (power cycle)

### Step 3: Test Individual Halves

**Test RIGHT half only:**
1. Power on only the right half
2. Look for "compression 4c" in Bluetooth devices
3. Pair and test typing
4. All keys on the right half should work

**Test LEFT half connection:**
1. With right half paired and working
2. Power on left half
3. Left half should automatically connect to right half
4. Test that left half keys work through the right half

### Step 4: Common Issues & Solutions

**Issue: Keyboard not discoverable**
- Solution: Flash firmware again, make sure you're using right half
- Verify: `CONFIG_ZMK_BLE=y` is set (it is in our config)

**Issue: Pairs but doesn't type**
- Solution: Clear bonds on BOTH computer and keyboard
- Try: Power cycle both halves

**Issue: Only one half works**
- Solution: Make sure both halves have matching firmware
- Check: Left half automatically connects to right half (no manual pairing needed)

**Issue: Intermittent connection**
- Solution: Check power management settings
- Verify: Controllers are fully charged

### Step 5: Advanced Debugging

If still not working, you can enable logging:

1. Add to your config override:
```
CONFIG_LOG=y
CONFIG_ZMK_LOG_LEVEL_DBG=y
CONFIG_BT_DEBUG_LOG=y
```

2. Connect via serial/USB to see debug output

## Key Combinations for Bluetooth Control

| Action | Key Combination |
|--------|----------------|
| Enter Bluetooth Layer | Space + Enter |
| Clear All Bonds | Top-left + Top-right (Grave + Minus) |
| Select Device 0 | BT Layer + 1 key |
| Select Device 1 | BT Layer + 2 key |
| Select Device 2 | BT Layer + 3 key |
| Select Device 3 | BT Layer + 4 key |
| Select Device 4 | BT Layer + 5 key |
| Clear Bonds | BT Layer + leftmost or rightmost key |

## Expected Behavior

1. **Right half powers on** → Becomes discoverable as "compression 4c"
2. **You pair with right half** → Can type with right half keys
3. **Left half powers on** → Automatically connects to right half
4. **Both halves work** → Full keyboard functionality

## Still Not Working?

If none of the above helps:

1. **Verify firmware files** - Make sure you flashed the correct .uf2 files
   - `4c_right` firmware to the right controller
   - `4c_left` firmware to the left controller

2. **Test with different device** - Try pairing with phone/tablet instead of computer

3. **Hardware check** - Verify controllers are working (LED indicators, power)

4. **Build fresh firmware** - Sometimes a clean rebuild helps

The most common issue is old pairing data, so start with clearing Bluetooth bonds!