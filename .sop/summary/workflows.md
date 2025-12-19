# Workflows & Logic Flows

## Main Excess Charging Workflow

### Primary Control Loop
**Trigger:** Every 30 seconds (`Time cron "*/30 * * * * ?"`)

```mermaid
flowchart TD
    A[Timer Trigger: 30s] --> B{Exit Conditions Check}
    B -->|Car Not Connected| Z[Exit]
    B -->|Excess Charging OFF| Z
    B -->|Pass| C[Calculate Available Power]
    
    C --> D[Update Timestamps]
    D --> E[Check Time Constraints]
    E --> F{Sufficient Time Elapsed?}
    F -->|No| Z
    F -->|Yes| G[Evaluate Power Thresholds]
    
    G --> H{Current Charging State}
    H -->|Charging| I[Charging Active Logic]
    H -->|Stopped| J[Charging Stopped Logic]
    
    I --> K[Current Adjustment]
    I --> L[Phase Switching Logic]
    
    J --> M[Start Charging Logic]
    
    K --> N[Apply Changes]
    L --> N
    M --> N
    N --> O[Update State Timestamps]
    O --> Z
```

## Power Calculation Workflow

### Available Power Determination
```mermaid
flowchart LR
    A[Grid Power Reading] --> C[Power Calculation]
    B[Current Charger Power] --> C
    C --> D[Available Power = -Grid + Charger]
    D --> E[Update AvailablePowerCharger Item]
    
    subgraph "Example Calculation"
        F[Grid: -1500W export] --> H[Available: 2500W]
        G[Charger: 1000W] --> H
    end
```

### Power Threshold Logic
```mermaid
flowchart TD
    A[Available Power: 2500W] --> B{Power Evaluation}
    B -->|< 1180W| C[Insufficient - Stop Countdown]
    B -->|1180W - 1580W| D[Single Phase Range]
    B -->|1580W - 3940W| E[Single Phase Optimal]
    B -->|3940W - 4340W| F[Phase Switch Range]
    B -->|> 4340W| G[Three Phase Optimal]
    
    C --> H[Charging Stop Logic]
    D --> I[Maintain Single Phase]
    E --> I
    F --> J[Phase Switch Evaluation]
    G --> K[Switch to Three Phase]
```

## Countdown Timer Workflows

### Charging Stop Countdown
```mermaid
sequenceDiagram
    participant Rule as Rule Engine
    participant Timer as Stop Countdown
    participant Charger as Go-E Charger
    
    Rule->>Rule: Detect Low Power
    Rule->>Timer: Start Countdown (4 cycles)
    
    loop Every 30 seconds
        Rule->>Timer: Decrement Counter
        Timer-->>Rule: Cycles Remaining
        alt Counter > 1
            Rule->>Rule: Continue Monitoring
        else Counter = 1
            Rule->>Charger: Stop Charging
            Rule->>Timer: Reset to 0
        end
    end
```

### Phase Change Countdown
```mermaid
sequenceDiagram
    participant Rule as Rule Engine
    participant Timer as Phase Countdown
    participant Charger as Go-E Charger
    
    Rule->>Rule: Detect Phase Switch Condition
    Rule->>Timer: Start Countdown (6 cycles)
    
    loop Every 30 seconds
        Rule->>Timer: Decrement Counter
        Timer-->>Rule: Cycles Remaining
        alt Counter > 1
            Rule->>Rule: Wait for Countdown
        else Counter = 1
            Rule->>Charger: Switch Phases
            Rule->>Timer: Reset to 0
        end
    end
```

## Current Adjustment Workflow

### Dynamic Current Calculation
```mermaid
flowchart TD
    A[Available Power: 2500W] --> B[Get Current Phases]
    B --> C{Phase Count}
    C -->|1 Phase| D[Calculate: 2500W ÷ 230V = 10.87A]
    C -->|3 Phase| E[Calculate: 2500W ÷ (230V × 3) = 3.62A]
    
    D --> F[Apply Limits: min(max(10.87, 6), 16) = 10A]
    E --> G[Apply Limits: min(max(3.62, 6), 16) = 6A]
    
    F --> H{Current Change > 1A?}
    G --> H
    H -->|Yes| I[Send New Current Command]
    H -->|No| J[No Change Needed]
```

### Current Hysteresis Logic
```mermaid
graph LR
    A[Current Amps: 8A] --> B[Target Current: 10A]
    B --> C{Difference ≥ 1A?}
    C -->|Yes: 2A difference| D[Update to 10A]
    C -->|No: <1A difference| E[Keep Current Setting]
    
    F[Current Amps: 10A] --> G[Target Current: 10.5A]
    G --> H{Difference ≥ 1A?}
    H -->|No: 0.5A difference| I[Keep 10A]
```

## Phase Switching Workflow

### Single to Three Phase Transition
```mermaid
sequenceDiagram
    participant Rule as Rule Engine
    participant Power as Power Monitor
    participant Timer as Phase Timer
    participant Charger as Go-E Charger
    
    Power->>Rule: Available Power > 4340W
    Rule->>Rule: Check Current Phase = 1
    Rule->>Rule: Check Time Since Last Change > 500s
    
    alt First Detection
        Rule->>Timer: Start Countdown (6 cycles)
    else Countdown Active
        Rule->>Timer: Decrement Counter
        alt Counter = 1
            Rule->>Charger: Switch to 3 Phases
            Rule->>Rule: Sleep 2000ms
            Rule->>Rule: Update Timestamps
        end
    end
```

### Three to Single Phase Transition
```mermaid
sequenceDiagram
    participant Rule as Rule Engine
    participant Power as Power Monitor
    participant Timer as Phase Timer
    participant Charger as Go-E Charger
    
    Power->>Rule: Available Power < 3940W
    Rule->>Rule: Check Current Phase = 3
    Rule->>Rule: Check Time Since Last Change > 500s
    
    alt First Detection
        Rule->>Timer: Start Countdown (6 cycles)
    else Countdown Active
        Rule->>Timer: Decrement Counter
        alt Counter = 1
            Rule->>Charger: Switch to 1 Phase
            Rule->>Rule: Sleep 1000ms
            Rule->>Rule: Update Timestamps
        end
    end
```

## Manual Override Workflow

### Manual Charging Control
```mermaid
flowchart TD
    A[Manual Switch Changed] --> B{Excess Charging State}
    B -->|OFF| C[Process Manual Command]
    B -->|ON| D[Ignore Manual Command]
    
    C --> E{Manual Switch State}
    E -->|ON| F[Enable Manual Charging]
    E -->|OFF| G[Disable Manual Charging]
    
    F --> H[Set Force State = 2]
    F --> I[Start Transaction]
    F --> J[Send Notification]
    
    G --> K[Set Force State = 1]
    G --> L[Send Notification]
```

## Error Handling Workflows

### System State Validation
```mermaid
flowchart TD
    A[Rule Execution Start] --> B{PWM Signal Check}
    B -->|UNDEF| C[Exit - No Connection]
    B -->|READY_NO_CAR| C
    B -->|IDLE| C
    B -->|Valid State| D{Excess Charging Check}
    
    D -->|OFF| E[Exit - Mode Disabled]
    D -->|ON| F[Continue Processing]
    
    F --> G{Item State Validation}
    G -->|NULL States Found| H[Initialize Items]
    G -->|All Valid| I[Proceed with Logic]
    
    H --> I
```

### Timestamp Initialization Workflow
```mermaid
flowchart LR
    A[Check LastStateChangeTime] --> B{State = NULL?}
    B -->|Yes| C[Initialize with Current Time]
    B -->|No| D[Use Existing Value]
    
    E[Check LastPhaseChangeTime] --> F{State = NULL?}
    F -->|Yes| G[Initialize with Current Time]
    F -->|No| H[Use Existing Value]
    
    C --> I[Continue Processing]
    D --> I
    G --> I
    H --> I
```

## Notification Workflows

### State Change Notifications
```mermaid
sequenceDiagram
    participant Rule as Rule Engine
    participant Email as Email Service
    participant User as User
    
    Rule->>Rule: Detect State Change
    Rule->>Email: Send Notification
    Email->>User: Email Alert
    
    Note over Rule,User: Notification Types:
    Note over Rule,User: - Charging Started/Stopped
    Note over Rule,User: - Phase Switch Events
    Note over Rule,User: - Manual Mode Changes
```

## Performance Optimization Workflows

### Execution Efficiency
```mermaid
flowchart TD
    A[30s Timer Trigger] --> B[Quick Exit Checks]
    B -->|Exit Conditions Met| C[Return Immediately]
    B -->|Continue| D[Essential Calculations Only]
    
    D --> E[Minimize API Calls]
    E --> F[Batch State Updates]
    F --> G[Single Notification Per Cycle]
    
    G --> H[Update Timestamps Last]
    H --> I[Complete Execution]
```

### State Change Throttling
```mermaid
timeline
    title State Change Timing Control
    
    section Time Constraints
        T+0s    : State Change Occurs
        T+30s   : Minimum Wait for Next State Change
        T+500s  : Minimum Wait for Phase Change
        
    section Countdown Delays
        Detection   : Start Countdown Timer
        Cycle 1-3   : Monitor and Decrement
        Cycle 4     : Execute Action
```
