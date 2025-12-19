# Contributing to OpenHAB Solar EV Charging

## Development Setup

### Prerequisites
- OpenHAB 4.x installation
- Go-E Genesis wallbox
- Fronius Gen24 inverter with Smart Meter
- Network connectivity to all devices

### Configuration
1. Copy `solar.items` to your OpenHAB `conf/items/` directory
2. Copy `solar.rules` to your OpenHAB `conf/rules/` directory
3. Configure thing files for your hardware IP addresses
4. Update email notification address in rules

## Code Style

### Rules DSL Guidelines
- Use descriptive variable names
- Comment complex logic sections
- Keep configuration constants at the top
- Use consistent indentation (4 spaces)

### Item Naming Convention
- Prefix with device type (GoECharger*, Meter_*, etc.)
- Use descriptive suffixes (Power, Current, State, etc.)
- Follow OpenHAB naming standards

## Testing

### Before Submitting Changes
1. Test with actual hardware setup
2. Verify all countdown timers work correctly
3. Test manual override functionality
4. Validate email notifications
5. Check logs for errors

### Safety Testing
- Test emergency stop scenarios
- Verify current limits are enforced
- Test phase switching stability
- Validate power calculation accuracy

## Documentation

- Update relevant files in `.sop/summary/` when making changes
- Keep README.md synchronized with major changes
- Document new configuration parameters
- Update workflow diagrams if logic changes

## Submitting Changes

1. Create feature branch from main
2. Make changes with clear commit messages
3. Test thoroughly with hardware
4. Update documentation
5. Submit pull request with description of changes
