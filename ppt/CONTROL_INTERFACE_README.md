# PPT Modulator Control Interface - Display Implementation

## Overview
Added comprehensive control interface to `ppt-modulator.bob` matching the reference design in `controlInterface.png`.

## Display Updates

### New Control Interface Section (Y: 480px, Height: 280px)
Full-width control panel (1180px) with all command buttons and HVPS voltage control.

### Layout Structure

#### Row 1 (Top Controls):
- **Thyratron Heater** (X: 15-150)
  - ON button → `$(P):$(R):Thy:OnCmd`
  - OFF button → `$(P):$(R):Thy:OffCmd`
  - Status LEDs for ON/OFF states

- **Klystron 80%** (X: 175-335)
  - ON button → `$(P):$(R):Klys:On80Cmd`
  - OFF button → `$(P):$(R):Klys:Off80Cmd`
  - Status LEDs for ON/OFF states

- **Klystron 100%** (X: 360-525)
  - ON button → `$(P):$(R):Klys:On100Cmd`
  - OFF button → `$(P):$(R):Klys:Off100Cmd`
  - Status LEDs for ON/OFF states

- **Charge PFN** (X: 550-690)
  - ON button → `$(P):$(R):ChargePFN:OnCmd`
  - OFF button → `$(P):$(R):ChargePFN:OffCmd`
  - Status LEDs for ON/OFF states

#### Row 2 (Bottom Controls):
- **Focus Power Supply** (X: 15-150)
  - ON button → `$(P):$(R):Focus:OnCmd`
  - OFF button → `$(P):$(R):Focus:OffCmd`
  - Status LEDs for ON/OFF states

- **Premagnetisation** (X: 175-355)
  - ON button → `$(P):$(R):Premag:OnCmd`
  - OFF button → `$(P):$(R):Premag:OffCmd`
  - Status LEDs for ON/OFF states

- **HVPS** (X: 385-525)
  - ON button → `$(P):$(R):HVPS:OnCmd`
  - OFF button → `$(P):$(R):HVPS:OffCmd`
  - Status LEDs for ON/OFF states

- **RESET** (X: 720-800)
  - RESET button (red) → `$(P):$(R):Reset:Cmd`

#### HVPS Voltage Section (X: 840-1120):
Separated by vertical line, contains:

- **HVPS raw** column (X: 860-980)
  - Text entry → `$(P):$(R):HVPS:VoltageSet` (editable)
  - Readback → `$(P):$(R):HVPS:VoltageSet:RBV`

- **HVPS** column (X: 1000-1120)
  - Actual voltage display → `$(P):$(R):HVPS:Voltage`
  - Duplicate readback → `$(P):$(R):HVPS:Voltage`

- **APPLY Button** (X: 920, Y: 200)
  - Blue button to commit voltage setpoint

- **Ready Indicator** (X: 1070-1150)
  - Green LED → `$(P):$(R):Ready`

### Updated Section Positions

Due to the new control interface (280px height), all subsequent sections were shifted down:

| Section | Old Y | New Y | Height |
|---------|-------|-------|--------|
| Timers Group | 480 | 770 | 180 |
| Thyratron Status | 500 | 960 | 270 |
| Klystron Status | 500 | 960 | 480 |
| Focus Status | 780 | 1240 | 480 |
| Premag Status | 990 | 1450 | 270 |
| Footer | 1370 | 1720 | - |

**New Display Height**: 1750px (was 1400px)

## Control Button Features

### Action Buttons
All control buttons use `action_button` widget with `write_pv` action:
- **ON buttons**: Dark green background (#008000), white text
- **OFF buttons**: Bright green background (#00FF00), black text
- **RESET button**: Dark red background (#B40000), white text
- **APPLY button**: Blue background (#0064B4), white text

All buttons are **momentary** - they write value `1` and rely on the auto-reset mechanism in `ppt_control.template`.

### Status LEDs
Each control section has LED indicators showing current state:
- Dark green (off state): RGB(60, 100, 60)
- Bright green (on state): RGB(0, 255, 0)
- Labels show "ON" / "OFF"

## PV Connections

### Command PVs (Write)
```
$(P):$(R):Thy:OnCmd          - Thyratron heater ON
$(P):$(R):Thy:OffCmd         - Thyratron heater OFF
$(P):$(R):Klys:On80Cmd       - Klystron 80% ON
$(P):$(R):Klys:On100Cmd      - Klystron 100% ON
$(P):$(R):Klys:Off80Cmd      - Klystron 80% OFF
$(P):$(R):Klys:Off100Cmd     - Klystron 100% OFF
$(P):$(R):Focus:OnCmd        - Focus power supply ON
$(P):$(R):Focus:OffCmd       - Focus power supply OFF
$(P):$(R):Premag:OnCmd       - Premagnetisation ON
$(P):$(R):Premag:OffCmd      - Premagnetisation OFF
$(P):$(R):HVPS:OnCmd         - HVPS ON
$(P):$(R):HVPS:OffCmd        - HVPS OFF
$(P):$(R):ChargePFN:OnCmd    - Charge PFN ON
$(P):$(R):ChargePFN:OffCmd   - Charge PFN OFF
$(P):$(R):Reset:Cmd          - System reset
$(P):$(R):HVPS:VoltageSet    - HVPS voltage setpoint (kV)
```

### Status PVs (Read)
```
$(P):$(R):Thy:HeaterOn       - Thyratron heater ON status
$(P):$(R):Thy:HeaterOff      - Thyratron heater OFF status
$(P):$(R):Klys:Heater80On    - Klystron 80% ON status
$(P):$(R):Klys:Heater80Off   - Klystron 80% OFF status
$(P):$(R):Klys:Heater100On   - Klystron 100% ON status
$(P):$(R):Klys:Heater100Off  - Klystron 100% OFF status
$(P):$(R):Focus:PowerOn      - Focus power supply ON status
$(P):$(R):Focus:PowerOff     - Focus power supply OFF status
$(P):$(R):Premag:PowerOn     - Premagnetisation ON status
$(P):$(R):Premag:PowerOff    - Premagnetisation OFF status
$(P):$(R):HVPS:PowerOn       - HVPS ON status
$(P):$(R):HVPS:PowerOff      - HVPS OFF status
$(P):$(R):ChargePFN:On       - Charge PFN ON status
$(P):$(R):ChargePFN:Off      - Charge PFN OFF status
$(P):$(R):HVPS:Voltage       - HVPS actual voltage (kV)
$(P):$(R):HVPS:VoltageSet:RBV - HVPS voltage setpoint readback
$(P):$(R):Ready              - System ready indicator
```

## HVPS Voltage Control

The voltage control section provides:
1. **Setpoint Entry**: Direct text input in kV (0.0 - 50.0)
2. **Setpoint Readback**: Confirmation of commanded value
3. **Actual Voltage**: Current HVPS voltage measurement
4. **APPLY Button**: Commits the setpoint (optional - may auto-apply on entry)

Engineering units conversion:
- Display: kV (0.0 - 50.0)
- Device: Raw units 0-500 (factor of 10)
- Example: 45.5 kV displayed = 455 device units

## Integration with ppt_control.template

The control interface requires:
1. **ppt_control.template** loaded in IOC
2. Status PVs defined in monitoring database
3. Auto-reset mechanism (500ms) ensures edge-triggered commands

## Display Macros

Default macros defined at display level:
```xml
<macros>
  <P>PPT</P>
  <R>MOD1</R>
</macros>
```

Override when opening display:
```
P=LAB:, R=PPT1:
```

## Visual Design

Matches reference design (`controlInterface.png`):
- ✅ ON/OFF buttons with green color scheme
- ✅ Status LED indicators below each control
- ✅ HVPS voltage entry/readback fields
- ✅ RESET button (red warning color)
- ✅ Ready indicator LED
- ✅ APPLY button for voltage commitment
- ✅ Vertical separator line for HVPS section
- ✅ Consistent button sizing (60x35 for ON/OFF)
- ✅ Professional gray background (#F0F0F0)

## XML Validation

Display file validated successfully with `xmllint`.

## Testing

Before using in production:
1. Verify all command PVs exist in IOC
2. Test each ON/OFF command individually
3. Confirm auto-reset behavior (commands reset after 500ms)
4. Validate HVPS voltage limits (0-50 kV)
5. Check status LED connections
6. Test RESET command functionality
7. Verify Ready indicator logic

## Notes

- All control buttons are momentary (pulse mode)
- Status LEDs should reflect actual hardware state from status PVs
- HVPS voltage control requires proper engineering unit conversion in database
- The layout is optimized for 1200px width displays
- Total display height now 1750px to accommodate control interface
