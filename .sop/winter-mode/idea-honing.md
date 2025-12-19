# Requirements Clarification - Winter Mode

This document captures the detailed requirements through Q&A clarification.

---

## Q1: Mode Selection and Priority

**Question:** How should Winter Mode relate to your existing Excess Charging and Manual modes? Should it be:
- A completely separate mode (like a third switch/option)
- A sub-mode that activates automatically when Excess Charging is enabled but solar is too low
- A seasonal override that replaces Excess Charging during winter months

What's your preferred approach for selecting and activating Winter Mode?

**Answer:** Separate mode, named "Minimal Excess Charging" or similar - a third independent charging option alongside the existing Excess Charging and Manual modes.

---

## Q2: Mode Exclusivity and Interaction

**Question:** How should the three modes interact with each other? Should they be:
- Mutually exclusive (only one can be active at a time)
- Allow Minimal Excess Charging to work alongside Manual mode
- Have a priority order when multiple modes are enabled

What behavior do you want when a user has multiple modes enabled simultaneously?

**Answer:** The minimal charging mode could be an extension of excess charging - so it would work as part of the existing Excess Charging mode rather than being completely separate.

---

## Q3: Threshold Behavior and Hysteresis

**Question:** For the 500W threshold, should there be hysteresis to prevent rapid on/off switching? For example:
- Start charging at 500W, stop at 400W (100W hysteresis)
- Start charging at 500W, stop at 450W (50W hysteresis)
- Use exact 500W threshold with no hysteresis

What threshold behavior would work best for stability?

**Answer:** Start charging at 600W and stop at 300W (300W hysteresis for maximum stability).

---

## Q4: Charging Current Behavior

**Question:** When minimal excess charging is active, should it:
- Always charge at exactly 6A (minimum current)
- Dynamically adjust current based on available solar (like regular excess charging, but starting from 600W)
- Use a fixed current that you can configure (e.g., 6A, 8A, or 10A)

What charging current behavior do you prefer for the minimal mode?

**Answer:** Dynamically adjust - between 600W and 1380W it would charge with 6A, and if more power is available it would go higher (seamlessly transitioning to normal excess charging behavior).

---

## Q5: Phase Switching Behavior

**Question:** Should the minimal excess charging mode support phase switching (1-phase to 3-phase) like the regular excess charging, or should it:
- Stay in 1-phase only (simpler, appropriate for lower power levels)
- Use the same phase switching logic as regular excess charging (at 4140W threshold)
- Have its own phase switching threshold optimized for winter conditions

What phase behavior makes sense for minimal excess charging?

**Answer:** It would be nice to have a main switch to enable or disable phase switching anyway which is true for all modes - a global phase switching control that affects both regular and minimal excess charging.

---

## Q6: User Interface and Control

**Question:** How should users control the minimal excess charging feature? Should it be:
- An additional switch item (like "MinimalExcessCharging" alongside "ExcessCharging")
- A configuration parameter within the existing excess charging (like a "winter mode" toggle)
- Automatic activation based on conditions (no user control needed)

What user interface approach would work best for you?

**Answer:** An additional switch item - a separate control alongside the existing ExcessCharging switch.

---

## Q7: Mode Priority and Conflict Resolution

**Question:** When both "ExcessCharging" and "MinimalExcessCharging" switches are enabled, what should happen?
- MinimalExcessCharging takes priority (extends the range down to 600W)
- ExcessCharging takes priority (ignores minimal mode)
- They work together seamlessly (minimal fills the 600W-1380W gap, regular handles 1380W+)
- Prevent both from being enabled simultaneously

What behavior makes the most sense for your use case?

**Answer:** They work seamlessly together, which is the same as option 1 - extends the range down to 600W when both are enabled.

---

## Q8: Countdown Timer Behavior

**Question:** Should the minimal excess charging use the same countdown timers as regular excess charging for stability, or should it have different timing?
- Use same countdown timers (4 cycles to start/stop, 6 cycles for phase changes)
- Use shorter countdown timers (since it's lower power, maybe 2-3 cycles)
- Use longer countdown timers (for extra stability in variable winter conditions)
- No countdown timers (immediate response)

What timing behavior would work best for the 600W/300W thresholds?

**Answer:** Use the same countdown timers as regular excess charging (4 cycles to start/stop, 6 cycles for phase changes).

---

## Q9: Global Phase Switching Control

**Question:** For the global phase switching control you mentioned, how should it work?
- A switch that completely disables all phase switching (stays 1-phase only when OFF)
- A switch that allows/prevents automatic phase switching but allows manual phase control
- Should it affect both excess charging modes equally?
- What should be the default state (phase switching enabled or disabled)?

**Answer:** A switch that completely disables all phase switching (stays 1-phase only when OFF) and affects both excess charging modes equally. Default state: allow phase switching (switch ON).
