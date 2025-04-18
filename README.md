# Voltage Window Monitor

This package defines an atopile component for a voltage window monitor based on the ATL431LI shunt regulator.

See the [ATL431 Datasheet System Examples (Figure 37)](https://www.ti.com/lit/ds/symlink/atl431li.pdf?ts=1744777182166) for more details

```mermaid
graph TD
    subgraph "Voltage Window Monitor Concept (from README)"
        A[Input Voltage Vin] --> B{High Threshold Check}
        A --> C{Low Threshold Check}

        subgraph "High Threshold Path"
            direction LR
            B --> D[Voltage Divider R1A, R2A]
            D --> E[Comparator 1 (ATL431)]
            E -- Compare with Vref --> F{Vin > V_high?}
        end

        subgraph "Low Threshold Path"
            direction LR
            C --> G[Voltage Divider R1B, R2B]
            G --> H[Comparator 2 (ATL431)]
            H -- Compare with Vref --> I{Vin < V_low?}
        end

        F --> J{Output Logic}
        I --> J
        J --> K[Final Output (High if Vin is within window, Low otherwise)]
    end
```

## Threshold Equations

The high and low voltage thresholds are set using external resistor dividers connected to the REF pin of two separate ATL431LI devices (or similar comparators with an internal reference). The threshold is triggered when the voltage at the REF pin equals the internal reference voltage (`Vref`, typically 2.5V).

- **High Threshold (V_high):**

  ```
  V_high = Vref * (1 + R1A / R2A)
  ```

  Where R1A and R2A form the voltage divider for the high threshold comparator.

- **Low Threshold (V_low):**

  ```
  V_low = Vref * (1 + R1B / R2B)
  ```

  Where R1B and R2B form the voltage divider for the low threshold comparator.

## Todo

- Automatically solve resistor values with assertions given Vin, Ika_min, Iref_min, etc
