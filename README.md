# Adlar Heatpump (Aurora II) — Home Assistant Integration

A HACS-compatible custom integration for the **Adlar Aurora II** heat pump, communicating over **Modbus TCP** (e.g. via an Elfin EW11 RS485-to-WiFi gateway).

No YAML required. All setup is done through the Home Assistant UI.

---

## Hardware setup

This integration assumes:

- An **RS485 splitter** (e.g. JPX-3002) with:
  - **Slave port** → Modbus cable from the heat pump
  - **Master 1** → Jan (JAN module / original controller)
  - **Master 2** → Elfin EW11 (RS485 to WiFi/TCP bridge)
- The **Elfin EW11** is configured in TCP Server mode, port **502** (or another port you choose)
- Modbus slave ID of the heat pump is **1** (default; change in setup if different)

---

## Installation

### Via HACS (recommended)

1. Open **HACS → Integrations**
2. Click the three-dot menu → **Custom repositories**
3. Add this repository URL, category: **Integration**
4. Search for **Adlar Heatpump** and install
5. Restart Home Assistant

### Manual

Copy the `custom_components/adlar_heatpump/` folder into your HA `config/custom_components/` directory, then restart.

---

## Configuration

1. Go to **Settings → Devices & Services → Add Integration**
2. Search for **Adlar Heatpump**
3. Enter:
   - **IP address** of the Elfin EW11
   - **Port** (default `502`)
   - **Slave ID** (default `1`)
   - **Scan interval** in seconds (default `30`)

---

## Entities created

### Sensors (read-only)
| Name | Unit | Notes |
|---|---|---|
| Compressor Target Frequency | Hz | |
| Compressor Running Frequency | Hz | |
| Fan Running Speed | Hz | |
| EEV Open Step | P | |
| EVI Valve Open Step | P | |
| AC Input Voltage | V | |
| AC Input Current | A | ×0.1 |
| Compressor Phase Current | A | ×0.1 |
| Compressor IPM Temp | °C | |
| High/Low Pressure Saturation Temp | °C | |
| Ambient Temp T1 … Economizer T8/T9 | °C | Multiple temps |
| Plate HX Exhaust Temp | °C | |
| Water Pump Speed PWM | Hz | |
| Water Flow | L/min | |

### Binary Sensors (running status bits)
- Refrigerant Recovery, Anti-freeze (primary/secondary), Fault Alarm, Oil Return, Frosting, Shutdown (temp/fault), Unit Operation, Unit Waiting

### Controls
| Entity | Type | Options / Range |
|---|---|---|
| Heatpump ON/OFF | Switch | on / off |
| Temp Set Cooling | Number | 7–25 °C |
| Temp Set Floor Heating | Number | 20–60 °C |
| Mode | Select | Cooling / Heating / Floor Heating |
| Running Mode | Select | Standard / Boost / Silent |
| Cooling Setting Curve | Select | Off, H1–H8, L1–L8 |
| Heating Setting Curve | Select | Off, H1–H8, L1–L8 |
| Underfloor Heating Setting Curve | Select | Off, H1–H8, L1–L8 |

---

## Elfin EW11 configuration tips

- Set **Work Mode** to `TCP Server`
- Set **Local Port** to `502`
- Set **Baud Rate** to `9600`, **Data bits** `8`, **Stop bits** `1`, **Parity** `None`
- These match the heat pump's UART settings

---

## Disclaimer

This integration is community-developed and not affiliated with Adlar. Use at your own risk. Incorrect writes to control registers could affect heat pump operation.
