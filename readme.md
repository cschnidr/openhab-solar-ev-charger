# Solar Excess Charging Control Rule for Openhab 4

## Introduction

This project implements an intelligent excess charging control system for electric vehicles, integrating OpenHAB 4 with a Go-E Genesis wallbox and a Fronius Gen24 inverter. The system optimizes electric vehicle charging by utilizing surplus solar power, maximizing self-consumption while minimizing grid dependency.

Excess charging refers to the intelligent use of surplus solar energy for charging electric vehicles. When solar production exceeds household consumption, the system automatically directs this excess power to vehicle charging, ensuring optimal use of renewable energy and reducing electricity costs.

## System Overview

<img src="pv-excess-charging.svg" width="500" />

### Real-World Performance

The following diagrams demonstrate the system's operation in practice:

<img src="24012025.png" height="200" /> <img src="29012025.png" height="200" />

Solar production (orange) and charging correlation demonstrating optimized self-consumption (yellow).

## Key Features

- Dynamic charging current adjustment based on available solar power
- Intelligent switching between 1-phase and 3-phase charging
- Advanced stability control mechanisms
- Manual charging override option
- Real-time monitoring and notifications

## Technical Solutions

The system implements several sophisticated control mechanisms to ensure stable and efficient operation:

### 1. Stability Control
- Power and current hysteresis to prevent oscillation
- Countdown timers for major state changes
- Minimum time intervals between switching operations

### 2. Charging Optimization
- Automatic phase switching (1-phase/3-phase) based on available power
- Dynamic current adjustment within safe limits
- Configurable power thresholds and safety margins

### 3. Safety Features
- Comprehensive system state monitoring
- Fail-safe operation modes
- Automatic error detection and handling

## Configuration Parameters

Key configurable parameters include:
- Charging current limits (6-16A)
- Phase switching thresholds
- Hysteresis values
- Timer durations
- Notification settings

## System Requirements

- OpenHAB 4 installation
- Go-E Genesis wallbox
- Fronius Gen24 inverter
- Required OpenHAB plugins:
  - Go-E Charger binding
  - Fronius binding

## Installation

### Install Required Bindings
    Open OpenHAB settings
    Navigate to Settings → Things → Install
    Search and install:
        Go-E Charger binding
        Fronius binding

### Configure Items
    Copy the provided items file content to your OpenHAB items configuration
    The items file includes all necessary definitions for:
        Fronius inverter measurements
        Go-E charger controls
        System status variables
        Countdown timers

### Add Rule
    Copy the provided rule file to your OpenHAB rules directory
    Verify that all item references match your configuration
    The rule implements:
        Excess charging control logic
        Phase switching
        Safety timers
        Manual control options

### Configure Sitemap

    Add the following to your sitemap configuration:

[Insert placeholder for sitemap configuration]

    This provides the user interface for:
        Monitoring charging status
        Controlling manual charging
        Viewing power flows

