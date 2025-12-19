# Implementation Plan: Minimal Excess Charging Feature

## Implementation Checklist

- [x] Step 1: Add new control items to solar.items
- [x] Step 2: Add configuration constants to rule
- [x] Step 3: Update rule exit condition logic
- [x] Step 4: Implement dynamic threshold calculation
- [x] Step 5: Modify power evaluation logic
- [x] Step 6: Add global phase switching control
- [x] Step 7: Test basic functionality with minimal mode
- [x] Step 8: Test combined mode operation
- [x] Step 9: Test phase switching control
- [x] Step 10: Validate countdown timer behavior
- [x] Step 11: Test edge cases and error conditions
- [x] Step 12: Update documentation and commit changes

---

## Implementation Steps

### Step 1: Add New Control Items to solar.items
**Objective:** Add the two new switch items required for the feature

**Implementation:**
- Add `MinimalExcessCharging` switch item to solar.items
- Add `AllowPhaseSwitching` switch item to solar.items
- Follow existing naming conventions and German labels
- Ensure items are properly formatted

**Test Requirements:**
- Verify items appear in OpenHAB UI
- Test switch state changes via UI
- Confirm items maintain state across OpenHAB restarts

**Integration:** Items will be referenced in the rule modifications in subsequent steps

**Demo:** Two new switches visible in OpenHAB interface, can be toggled on/off, states persist correctly

---

### Step 2: Add Configuration Constants to Rule
**Objective:** Add the new threshold constants to the existing rule configuration section

**Implementation:**
- Add `minimalChargingStartThreshold = 600` constant
- Add `minimalChargingStopThreshold = 300` constant  
- Place constants with existing configuration values
- Add comments explaining the thresholds

**Test Requirements:**
- Rule compiles without errors
- Constants are accessible within rule scope
- Values can be easily modified for testing

**Integration:** Constants will be used in dynamic threshold calculation logic

**Demo:** Rule loads successfully with new constants, no compilation errors, ready for threshold logic

---

### Step 3: Update Rule Exit Condition Logic
**Objective:** Modify the rule exit condition to support both charging modes

**Implementation:**
- Replace single `ExcessCharging.state != ON` check
- Add logic to allow execution when either mode is enabled
- Maintain all other existing exit conditions (car connection, etc.)
- Preserve existing safety checks

**Test Requirements:**
- Rule executes when only ExcessCharging is ON
- Rule executes when only MinimalExcessCharging is ON  
- Rule executes when both modes are ON
- Rule exits when both modes are OFF

**Integration:** Enables rule to process minimal excess charging scenarios

**Demo:** Rule responds appropriately to different switch combinations, maintains existing safety behavior

---

### Step 4: Implement Dynamic Threshold Calculation
**Objective:** Add logic to determine effective thresholds based on active modes

**Implementation:**
- Add boolean check for MinimalExcessCharging state
- Calculate effective start/stop thresholds dynamically
- Ensure seamless transition between threshold ranges
- Maintain backward compatibility with existing behavior

**Test Requirements:**
- Correct thresholds when only regular mode active (1380W/1180W)
- Correct thresholds when only minimal mode active (600W/300W)
- Correct thresholds when both modes active (600W/300W extended range)

**Integration:** Provides threshold values for power evaluation logic

**Demo:** System calculates appropriate thresholds based on switch states, logs show correct values

---

### Step 5: Modify Power Evaluation Logic
**Objective:** Replace hardcoded thresholds with dynamic threshold variables

**Implementation:**
- Replace `singlePhasePowerLimit` references with `effectiveStartThreshold`
- Replace hardcoded stop threshold with `effectiveStopThreshold`
- Maintain existing countdown timer logic
- Preserve all existing power calculation methods

**Test Requirements:**
- Charging starts at correct thresholds for each mode
- Charging stops at correct thresholds for each mode
- Countdown timers work correctly with new thresholds
- Current adjustment logic functions properly

**Integration:** Core charging logic now responds to minimal excess charging thresholds

**Demo:** Charging activates at 600W in minimal mode, 1380W in regular mode, stops at appropriate thresholds

---

### Step 6: Add Global Phase Switching Control
**Objective:** Implement global control to enable/disable phase switching for all modes

**Implementation:**
- Wrap existing phase switching logic with AllowPhaseSwitching check
- Ensure control affects both regular and minimal charging modes
- Maintain existing phase switching behavior when enabled
- Force 1-phase operation when disabled

**Test Requirements:**
- Phase switching works normally when AllowPhaseSwitching is ON
- Phase switching is completely disabled when AllowPhaseSwitching is OFF
- Control affects both charging modes equally
- System stays in 1-phase when phase switching disabled

**Integration:** Global control overrides all automatic phase switching decisions

**Demo:** Phase switching can be globally enabled/disabled, affects all charging modes, system respects the setting

---

### Step 7: Test Basic Functionality with Minimal Mode
**Objective:** Validate minimal excess charging works independently

**Implementation:**
- Enable only MinimalExcessCharging switch
- Test charging activation at 600W threshold
- Test charging deactivation at 300W threshold
- Verify 6A current setting in 600W-1380W range

**Test Requirements:**
- Charging starts after countdown when solar ≥ 600W
- Charging stops after countdown when solar < 300W
- Current remains at 6A in minimal power range
- All safety mechanisms function correctly

**Integration:** Confirms minimal mode works as standalone feature

**Demo:** EV charges at 6A when solar production is 600W-1380W, stops cleanly below 300W

---

### Step 8: Test Combined Mode Operation
**Objective:** Validate seamless operation when both modes are enabled

**Implementation:**
- Enable both ExcessCharging and MinimalExcessCharging switches
- Test extended range operation (600W start, dynamic current above 1380W)
- Verify smooth transition at 1380W boundary
- Confirm no conflicts between modes

**Test Requirements:**
- Charging starts at 600W with both modes enabled
- Current increases dynamically above 1380W
- Seamless transition between 6A and dynamic current
- No oscillation or conflicts at boundaries

**Integration:** Demonstrates modes work together as designed

**Demo:** Extended range charging from 600W to maximum power, smooth current transitions, unified behavior

---

### Step 9: Test Phase Switching Control
**Objective:** Validate global phase switching control affects all modes

**Implementation:**
- Test phase switching with AllowPhaseSwitching ON (normal behavior)
- Test phase switching disabled with AllowPhaseSwitching OFF
- Verify control works with minimal mode, regular mode, and combined modes
- Confirm 1-phase operation when disabled

**Test Requirements:**
- Normal phase switching behavior when control enabled
- No phase switching when control disabled
- Control affects all charging modes equally
- System maintains 1-phase when switching disabled

**Integration:** Global control successfully overrides automatic phase decisions

**Demo:** Phase switching can be globally controlled, affects all charging scenarios, provides user control

---

### Step 10: Validate Countdown Timer Behavior
**Objective:** Ensure countdown timers work correctly with new thresholds

**Implementation:**
- Test start countdown with 600W threshold
- Test stop countdown with 300W threshold  
- Verify phase change countdown still functions
- Confirm timer stability prevents oscillation

**Test Requirements:**
- 4-cycle countdown before starting charging at 600W
- 4-cycle countdown before stopping charging at 300W
- 6-cycle countdown for phase changes (if enabled)
- No rapid switching or oscillation

**Integration:** Countdown system provides stability for new thresholds

**Demo:** Stable charging behavior with appropriate delays, no rapid switching, countdown timers prevent oscillation

---

### Step 11: Test Edge Cases and Error Conditions
**Objective:** Validate system handles edge cases and maintains safety

**Implementation:**
- Test with car disconnected/connected during operation
- Test with network interruptions to wallbox/inverter
- Test rapid power fluctuations around thresholds
- Verify manual override still functions correctly

**Test Requirements:**
- Graceful handling of car connection changes
- Proper error handling for communication failures
- Stable operation during power fluctuations
- Manual mode overrides automatic modes correctly

**Integration:** System maintains robustness and safety under all conditions

**Demo:** System handles edge cases gracefully, maintains safety, manual override works, no crashes or errors

---

### Step 12: Update Documentation and Commit Changes
**Objective:** Document the new feature and commit implementation

**Implementation:**
- Update README.md with new switch descriptions
- Add configuration examples for new items
- Update system documentation with new thresholds
- Commit changes with descriptive commit message

**Test Requirements:**
- Documentation accurately describes new functionality
- Configuration examples are correct and complete
- All changes are properly committed to git
- Implementation matches design specification

**Integration:** Feature is fully documented and version controlled

**Demo:** Complete feature documentation, clean git history, ready for production use and future maintenance

---

## Implementation Notes

### Development Environment
- Test changes in development OpenHAB instance first
- Use actual hardware for final validation
- Keep backup of working configuration

### Safety Considerations
- Always test manual override functionality
- Verify emergency stop procedures work
- Validate current limits are enforced
- Confirm countdown timers prevent rapid switching

### Performance Expectations
- No impact on existing rule execution time
- Minimal additional memory usage
- Same 30-second execution cycle maintained
