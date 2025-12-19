# Existing Code Analysis

## Current Implementation Structure

### Key Configuration Constants
```javascript
val int singlePhasePowerLimit = 1380      // Current minimum threshold
val int threePhasePowerLimit = 4140
val int powerHysteresis = 200             // Phase change hysteresis
val int chargingStopCountdownStart = 4    // 2 minutes
val int chargingStartCountdownStart = 4   // 2 minutes
```

### Current Logic Flow
1. **Exit Conditions Check**: `ExcessCharging.state != ON`
2. **Power Calculation**: `availablePower = (gridPower * -1) + chargerPower`
3. **Threshold Logic**: Uses `singlePhasePowerLimit = 1380W`
4. **Current Adjustment**: Dynamic between 6-16A
5. **Phase Switching**: At 4140W threshold

### Control Items (from solar.items)
```
Switch ExcessCharging              "Überschussladen"
Switch ManuellLaden                "Manuell Laden"
```

## Integration Points for Minimal Excess Charging

### Simple Approach - Extend Existing Logic
1. **Add new item**: `Switch MinimalExcessCharging "Minimal Überschussladen"`
2. **Modify exit condition**: Check both switches
3. **Add lower threshold logic**: 600W start, 300W stop
4. **Keep same countdown timers**: Reuse existing stability system

### Key Insight: Keep It Simple
- Extend existing rule rather than create new one
- Add minimal new items (just the switch)
- Reuse existing power calculation and countdown logic
- Modify threshold checks to support lower range

### Current Power Threshold Logic Location
The main threshold check happens around line 70-80 in the rule where it evaluates:
```javascript
if (availablePower < (singlePhasePowerLimit - powerHysteresis))
```

This is where we need to add the minimal excess charging logic.
