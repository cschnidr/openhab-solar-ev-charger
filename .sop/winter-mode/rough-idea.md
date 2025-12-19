# Rough Idea: Winter Mode Charging

## Problem Statement
During winter time, solar production is often around 1kW, but the current excess charging system requires a minimum of 1380W (6A × 230V) to start charging. This means that available solar energy in the 500W-1000W range goes unused for EV charging.

## Proposed Solution
Add a new charging mode called "Winter Mode" that:
- Activates when solar production reaches minimum 500W threshold
- Charges at minimal current (6A single-phase) when solar production is available
- Stops charging when solar production drops below the threshold
- Provides a way to utilize lower solar production during winter months

## Context
This is an enhancement to an existing OpenHAB 4 solar excess charging system that currently supports:
- Excess Charging mode (dynamic current adjustment, 1380W+ threshold)
- Manual Charging mode (user override)

The new Winter Mode would be a third charging option optimized for winter solar conditions.
