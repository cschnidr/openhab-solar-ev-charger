# Codebase Information

## Project Overview
**Name:** OpenHAB Solar Excess EV Charging Control  
**Type:** Home Automation System  
**Platform:** OpenHAB 4  
**Language:** OpenHAB Rules DSL  

## Technology Stack
- **Primary Language:** OpenHAB Rules DSL
- **Platform:** OpenHAB 4 Home Automation
- **Hardware Integration:** 
  - Go-E Genesis Wallbox
  - Fronius Gen24 Inverter
  - Fronius Smart Meter
- **Communication:** Modbus protocol

## File Structure
```
/
├── solar.rules          # Main excess charging control logic (10.2KB)
├── solar.items          # OpenHAB item definitions (6.5KB)
├── instructions.txt     # Documentation template (1.4KB)
├── README.md           # Project documentation (5.0KB)
├── pv-excess-charging.svg # System architecture diagram (52.6KB)
├── 24012025.png        # Performance chart example (283KB)
├── 29012025.png        # Performance chart example (259KB)
└── .sop/               # Documentation output directory
```

## Core Components
1. **Excess Charging Control Rule** - Main automation logic
2. **Manual Charging Control Rule** - Override functionality
3. **Item Definitions** - Hardware interface mappings
4. **Configuration Parameters** - Tunable system settings

## Supported Languages
- ✅ OpenHAB Rules DSL (Primary)
- ❌ Other languages not present in codebase

## Key Features Identified
- Dynamic charging current adjustment (6-16A)
- Automatic 1-phase/3-phase switching
- Countdown timers for stability
- Power hysteresis control
- Email notifications
- Manual override capability

## Dependencies
- OpenHAB 4 platform
- Go-E Charger binding
- Fronius binding
- Email notification service

## Architecture Pattern
Event-driven automation system with time-based triggers and state management.
