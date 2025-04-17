# Voltage Window Monitor

This package defines an atopile component for a voltage window monitor based on the ATL431LI shunt regulator.

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

## Conceptual Diagram (JSON Representation)

The following JSON data represents the conceptual blocks and connections for the voltage window monitor circuit:

```json
{
  "diagram_type": "conceptual_block",
  "title": "Voltage Window Monitor using ATL431LI",
  "components": [
    { "id": "vin", "label": "V_IN", "type": "input_voltage" },
    {
      "id": "divider_high",
      "label": "High Threshold Divider (R1A, R2A)",
      "type": "resistor_divider"
    },
    {
      "id": "comparator_high",
      "label": "ATL431LI (High Threshold)",
      "type": "comparator_reference"
    },
    {
      "id": "divider_low",
      "label": "Low Threshold Divider (R1B, R2B)",
      "type": "resistor_divider"
    },
    {
      "id": "comparator_low",
      "label": "ATL431LI (Low Threshold)",
      "type": "comparator_reference"
    },
    {
      "id": "output_high",
      "label": "High Threshold Output",
      "type": "output_signal"
    },
    {
      "id": "output_low",
      "label": "Low Threshold Output",
      "type": "output_signal"
    }
  ],
  "connections": [
    { "from": "vin", "to": "divider_high", "label": "Input" },
    { "from": "vin", "to": "divider_low", "label": "Input" },
    {
      "from": "divider_high",
      "to": "comparator_high",
      "label": "REF Pin Input"
    },
    { "from": "divider_low", "to": "comparator_low", "label": "REF Pin Input" },
    {
      "from": "comparator_high",
      "to": "output_high",
      "label": "Cathode Output"
    },
    { "from": "comparator_low", "to": "output_low", "label": "Cathode Output" }
  ],
  "notes": [
    "Each ATL431LI compares its REF pin input against its internal Vref (~2.5V).",
    "The Cathode output state changes when the REF pin voltage crosses Vref."
  ]
}
```
