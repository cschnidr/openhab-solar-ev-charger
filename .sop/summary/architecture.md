# System Architecture

## Overview
The OpenHAB Solar EV Charging system implements an event-driven architecture that optimizes electric vehicle charging using excess solar power. The system integrates multiple hardware components through OpenHAB's binding framework.

## High-Level Architecture

```mermaid
graph TB
    A[Solar Panels] --> B[Fronius Gen24 Inverter]
    B --> C[Fronius Smart Meter]
    C --> D[OpenHAB 4 Platform]
    D --> E[Go-E Genesis Wallbox]
    E --> F[Electric Vehicle]
    
    D --> G[Email Notifications]
    D --> H[Web Interface]
    
    subgraph "OpenHAB Core"
        I[Rules Engine]
        J[Items Registry]
        K[Bindings]
    end
    
    D --> I
    D --> J
    D --> K
```

## Component Integration

```mermaid
graph LR
    A[Fronius Binding] --> B[Power Measurements]
    C[Go-E Binding] --> D[Charging Control]
    E[Rules Engine] --> F[Decision Logic]
    
    B --> E
    D --> E
    F --> D
    
    G[Timer Service] --> E
    H[Notification Service] --> E
```

## Design Patterns

### 1. Event-Driven Control
- **Trigger:** Time-based cron expression (`*/30 * * * * ?`)
- **Pattern:** Reactive system responding to power availability changes
- **Benefits:** Real-time adaptation to solar conditions

### 2. State Machine Pattern
```mermaid
stateDiagram-v2
    [*] --> Idle
    Idle --> Evaluating: Timer Trigger
    Evaluating --> Charging: Sufficient Power
    Evaluating --> Waiting: Insufficient Power
    Charging --> Evaluating: Timer Trigger
    Waiting --> Evaluating: Timer Trigger
    Charging --> Stopped: Manual Override
    Stopped --> Charging: Manual Enable
```

### 3. Hysteresis Control
- **Purpose:** Prevent rapid switching between states
- **Implementation:** Power and current thresholds with different up/down values
- **Example:** 1380W ± 200W for single-phase switching

### 4. Countdown Timer Pattern
- **Purpose:** Stability control for major state changes
- **Implementation:** Configurable countdown cycles before action
- **Types:** Charging start/stop, phase switching

## Data Flow Architecture

```mermaid
flowchart TD
    A[Solar Production] --> B[Power Calculation]
    C[Grid Consumption] --> B
    D[Current Charging] --> B
    
    B --> E[Available Power]
    E --> F[Decision Engine]
    
    F --> G[Current Adjustment]
    F --> H[Phase Switching]
    F --> I[Start/Stop Control]
    
    G --> J[Wallbox Commands]
    H --> J
    I --> J
```

## Integration Patterns

### Hardware Abstraction Layer
- **Fronius Integration:** Modbus-based power measurements
- **Go-E Integration:** Direct API control for charging parameters
- **Abstraction:** OpenHAB items provide unified interface

### Configuration-Driven Design
- **Constants:** Centralized configuration at rule level
- **Flexibility:** Easy adjustment without code changes
- **Parameters:** Power limits, hysteresis values, timer durations

### Safety-First Architecture
- **Fail-Safe:** System defaults to safe states on errors
- **Validation:** Input validation and boundary checking
- **Monitoring:** Comprehensive logging and notifications

## Scalability Considerations

### Current Limitations
- Single wallbox support
- Fixed power calculation algorithm
- Hardcoded notification email

### Extension Points
- Multiple wallbox support via item groups
- Pluggable power calculation strategies
- Configurable notification channels
- Dynamic pricing integration potential

## Performance Characteristics
- **Execution Frequency:** Every 30 seconds
- **Response Time:** Near real-time (sub-minute)
- **Stability:** Countdown timers prevent oscillation
- **Efficiency:** Minimal computational overhead
