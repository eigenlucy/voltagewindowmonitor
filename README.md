# Voltage Window Monitor

This package defines an atopile component for a voltage window monitor based on the ATL431LI shunt regulator.

See the [ATL431 Datasheet System Examples (Figure 37)](https://www.ti.com/lit/ds/symlink/atl431li.pdf?ts=1744777182166) for more details

## Block Diagram

```
          +5V
           │
           │
┌──────────┴──────────┐
│                     │
│      ┌─────┐        │      ┌──────────────┐
└──────┤ R1A ├────────┼──────┤     REF      │
       └─────┘        │      │              │
                      │      │      A       │ ATL431 #1
       ┌─────┐        ├──────┤              │ (High Threshold)
┌──────┤ R1B ├────────┘      │      K       │
│      └─────┘               └──────┬───────┘
│                                   │
│      ┌─────┐                      │
└──────┤ R3  ├──────────────────────┘
       └─────┘                      │
                                    │
                             ┌──────┴───────┐
                             │     OUT      │───────▶ OUT
                             └──────┬───────┘
                                    │
       ┌─────┐                      │
┌──────┤ R2A ├──────────┐      ┌────┴─────────┐
│      └─────┘          │      │     REF      │
│                       │      │              │
│      ┌─────┐          └──────┤      A       │ ATL431 #2
└──────┤ R2B ├─────────────────┤              │ (Low Threshold)
       └─────┘          ┌──────┤      K       │
                        │      └──────┬───────┘
       ┌─────┐          │             │
┌──────┤ R4  ├──────────┘             │
│      └─────┘                        │
│                                     │
▼                                     │
GND───────────────────────────────────┴

                            OUT = HIGH in window, LOW outside
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

