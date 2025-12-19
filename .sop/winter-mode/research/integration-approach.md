# Integration Approach - Keep It Simple

## Minimal Changes Required

### 1. New Item Definitions
Add two new items to `solar.items`:
```
Switch MinimalExcessCharging       "Minimal Überschussladen"
Switch AllowPhaseSwitching         "Phasenumschaltung erlauben"
```

### 2. Rule Modification Strategy
Extend existing "Excess Charging Control" rule with minimal changes:

#### A. Update Exit Condition
```javascript
// Current
if (ExcessCharging.state != ON) { return }

// New - allow either mode
if (ExcessCharging.state != ON && MinimalExcessCharging.state != ON) { return }
```

#### B. Add Dynamic Threshold Logic
```javascript
// Determine effective threshold based on active modes
val int effectiveThreshold = if (MinimalExcessCharging.state == ON) 600 else 1380
val int effectiveStopThreshold = if (MinimalExcessCharging.state == ON) 300 else (1380 - 200)
```

#### C. Modify Power Evaluation
Replace hardcoded `singlePhasePowerLimit` with `effectiveThreshold`

#### D. Add Phase Switching Control
```javascript
// Wrap existing phase switching logic with global control
if (AllowPhaseSwitching.state == ON && secondsSinceLastPhaseChange >= minTimeBetweenPhaseChanges) {
    // existing phase switching logic here
}
```

### 3. Benefits of This Approach
- **Minimal code changes** - extends existing logic
- **Reuses all existing systems** - countdown timers, phase switching, notifications
- **Single rule maintenance** - no duplicate logic
- **Seamless integration** - both modes work together naturally
- **Global phase control** - affects all charging modes uniformly

### 4. Configuration Constants to Add
```javascript
val int minimalChargingStartThreshold = 600   // Start minimal charging
val int minimalChargingStopThreshold = 300    // Stop minimal charging
```

## Implementation Complexity: Very Low
- 2 new items (MinimalExcessCharging + AllowPhaseSwitching)
- ~15 lines of rule modifications
- Reuse 100% of existing infrastructure
