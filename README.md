# BIOMED-S3 — Custom ESP32-S3 Biomedical Board

> **Datasheet Rev 1.0 · April 2026**  
> A compact, high-performance biomedical microcontroller board built around the **Espressif ESP32-S3-WROOM-1** module, created by Jewzon Ravindra.

<img width="975" height="555" alt="image" src="https://github.com/user-attachments/assets/8cdf8f8a-3004-4d07-b6d2-4c2baedc2c6a" />


---

## Overview

The **BIOMED-S3** is designed for medical-grade sensor interfacing, data acquisition, and wireless transmission in portable and bench-top biomedical applications.

It integrates the **TCA9548APWR** 8-channel I2C multiplexer and **ADS1115IDGS** 16-bit ADC, enabling multiple I2C sensors to coexist on a single bus with precision analog front-end capability. Address configuration is user-selectable via onboard jumper pads.

| Core | Connectivity | Sensors | Interface |
|------|-------------|---------|-----------|
| ESP32-S3-WROOM-1 · 16MB Flash / 8MB PSRAM | Wi-Fi 802.11 b/g/n · Bluetooth 5 / BLE | TCA9548A I2C Mux · ADS1115 16-bit ADC | USB-C OTG · GPIO Headers |

---

## Key Features

- **ESP32-S3-WROOM-1** — Dual-core Xtensa LX7 @ 240 MHz, 16 MB Flash, 8 MB PSRAM
- **Wi-Fi** 802.11 b/g/n (2.4 GHz) + **Bluetooth 5.0 / BLE**
- **USB-C OTG** port (GPIO19/GPIO20) — firmware flashing, CDC serial, HID
- **TCA9548APWR** — 8-channel I2C switch, address configurable via jumper pads
- **ADS1115IDGS** — 16-bit, 4-channel ADC with PGA, address configurable via jumper pads
- **AMS1117-3.3** LDO voltage regulator
- **BOOT** and **RESET** buttons
- **Power** and **user** LEDs
- **Exposed GPIO headers** — all available GPIOs broken out

---

## Board Images



**Front**

<img width="387" height="320" alt="image" src="https://github.com/user-attachments/assets/edf96c13-6f1f-4ad0-9208-00c3778b1c64" />


**Back**

<img width="426" height="344" alt="image" src="https://github.com/user-attachments/assets/91dfeffd-3a35-4707-8e31-d5c667120ae6" />


---

## Electrical Specifications

### Absolute Maximum Ratings

> ⚠️ Exceeding these values may permanently damage the device.

| Parameter | Symbol | Min | Max | Unit |
|-----------|--------|-----|-----|------|
| Supply voltage (VIN) | VIN | -0.3 | 6.0 | V |
| 3.3V rail voltage | VDD | -0.3 | 3.6 | V |
| GPIO pin voltage | VGPIO | -0.3 | 3.6 | V |
| Storage temperature | TSTG | -40 | +125 | °C |

### Recommended Operating Conditions

| Parameter | Min | Typ | Max | Unit |
|-----------|-----|-----|-----|------|
| Input supply voltage (USB-C) | 4.5 | 5.0 | 5.5 | V |
| 3.3V output (AMS1117) | 3.235 | 3.3 | 3.365 | V |
| Operating temperature | -20 | 25 | +85 | °C |
| Operating humidity (non-condensing) | 10 | — | 90 | % RH |
| Maximum current (3.3V rail) | — | — | 800 | mA |

### Power Consumption

| Mode | Typical Current | Notes |
|------|----------------|-------|
| Active (Wi-Fi TX) | ~240 mA | Peak during transmission |
| Active (CPU only) | ~80 mA | No RF, full clock |
| Modem Sleep | ~20 mA | CPU active, Wi-Fi off |
| Deep Sleep | ~10 µA | RTC only |

---

## Pin Description

### USB-C Connector (J1)

| Pin | Name | Type | Description |
|-----|------|------|-------------|
| A4/B4 | VBUS | Power | 5V USB supply input |
| A5/B5 | CC1/CC2 | Config | 5.1kΩ pull-down for USB-C device mode |
| A6 | D+ | USB Data | Connected to GPIO20 (USB_D+) |
| A7 | D- | USB Data | Connected to GPIO19 (USB_D-) |
| A1/B1 | GND | Ground | Shield and signal ground |

### GPIO Header Pinout (J2 & J5)

All available GPIOs from the ESP32-S3-WROOM-1 module are exposed on dual-row headers. Pins reserved for SPI Flash/PSRAM (GPIO35–37) are not available for user I/O.

| GPIO | Alt Function | Direction | Notes |
|------|-------------|-----------|-------|
| GPIO0 | BOOT | Input | Strapping pin — hold LOW for download mode |
| GPIO1–7 | ADC / Touch | I/O | General purpose, analog capable |
| GPIO8 | I2C SDA | I/O | Default I2C data — TCA9548A & ADS1115 |
| GPIO9 | I2C SCL | I/O | Default I2C clock — TCA9548A & ADS1115 |
| GPIO19 | USB_D- | USB | USB OTG D- — do not use for GPIO when USB active |
| GPIO20 | USB_D+ | USB | USB OTG D+ — do not use for GPIO when USB active |
| GPIO43 | U0TXD | Output | UART0 TX — debug serial |
| GPIO44 | U0RXD | Input | UART0 RX — debug serial |
| GPIO45 | VSPI | Strapping | Strapping pin — avoid external pull on boot |
| GPIO46 | — | Strapping | Strapping pin — avoid external pull on boot |
| ENB1 | Enable pin | Selector | To enable TCA9548A |
| ENB2 | Enable pin | Selector | To enable ADS1115 |

<img width="892" height="922" alt="image" src="https://github.com/user-attachments/assets/4de101bd-0e2a-46d7-b22e-e402ddf35578" />

---

## Onboard Peripherals

### TCA9548APWR — 8-Channel I2C Multiplexer

The TCA9548APWR provides 8 independent I2C channels, enabling multiple sensors sharing the same I2C address without conflicts. Connected to the ESP32-S3 via the default I2C bus (GPIO8/GPIO9).

| Parameter | Value | Notes |
|-----------|-------|-------|
| Default I2C Address | 0x70 | A2=A1=A0=GND |
| Address Range | 0x70 – 0x77 | Set via jumper pads A0, A1, A2 |
| Number of Channels | 8 (SC0–SC7 / SD0–SD7) | Software selectable |
| Supply Voltage | 3.3V | Powered from onboard 3.3V rail |
| I2C Speed | Up to 400 kHz | Fast mode supported |
| Enable Pin | ENB2 | Enabled by default |

#### Address Jumper Pads (TCA9548A)

Three solder jumper pads (A0, A1, A2) on the bottom of the PCB control the I2C address. Bridge a pad → VCC (logic HIGH). Leave open → GND (logic LOW).

| A2 | A1 | A0 | I2C Address |
|----|----|----|-------------|
| Open (0) | Open (0) | Open (0) | 0x70 (Default) |
| Open (0) | Open (0) | Bridged (1) | 0x71 |
| Open (0) | Bridged (1) | Open (0) | 0x72 |
| Bridged (1) | Open (0) | Open (0) | 0x74 |
| Bridged (1) | Bridged (1) | Bridged (1) | 0x77 |

<img width="379" height="427" alt="image" src="https://github.com/user-attachments/assets/71280a27-9382-449f-a601-34f7d434e061" />


---

### ADS1115IDGS — 16-Bit ADC

A precision, low-power 16-bit ADC with programmable gain amplifier (PGA) and 4 single-ended / 2 differential input channels. Suitable for ECG, EMG, temperature, and pressure sensor acquisition.

| Parameter | Value | Notes |
|-----------|-------|-------|
| Resolution | 16-bit | ±32768 counts |
| Input Channels | 4 single-ended / 2 differential | AIN0–AIN3 |
| PGA Range | ±0.256V to ±6.144V | 6 gain settings |
| Data Rate | 8 – 860 SPS | Software configurable |
| Default I2C Address | 0x48 | ADDR pin = GND |
| Address Range | 0x48 – 0x4B | Set via ADDR jumper pad |
| Supply Voltage | 3.3V | Powered from onboard 3.3V rail |
| Enable Pin | ENB1 | Enabled by default |

#### Address Jumper Pad (ADS1115)

| ADDR Jumper Connection | I2C Address |
|------------------------|-------------|
| GND (Default — open pad) | 0x48 |
| VDD (Bridge to 3.3V) | 0x49 |
| SDA (Bridge to SDA line) | 0x4A |
| SCL (Bridge to SCL line) | 0x4B |


---

### Power Regulation — AMS1117-3.3

The AMS1117-3.3 LDO converts the 5V USB-C input to a stable 3.3V supply for the ESP32-S3 and all onboard peripherals.

| Parameter | Value | Notes |
|-----------|-------|-------|
| Input Voltage | 4.75V – 5.5V | From USB VBUS |
| Output Voltage | 3.3V ± 1% | Fixed |
| Max Output Current | 800 mA | With adequate heatsinking |
| Dropout Voltage | ~1.1V @ 800mA | |

---

## I2C Bus Architecture

The board uses a single I2C master bus from the ESP32-S3, shared by both the TCA9548A multiplexer and the ADS1115 ADC.

| Signal | ESP32-S3 Pin | Pull-up | Notes |
|--------|-------------|---------|-------|
| SDA | GPIO8 | 4.7kΩ to 3.3V | Shared bus |
| SCL | GPIO9 | 4.7kΩ to 3.3V | Shared bus |

### Default I2C Device Address Map

| Device | Default Address | Configurable? | Via |
|--------|----------------|---------------|-----|
| TCA9548APWR | 0x70 | Yes (0x70–0x77) | Jumper pads A0–A2 |
| ADS1115IDGS | 0x48 | Yes (0x48–0x4B) | Jumper pad ADDR |


---

## USB-C OTG Interface

The board exposes the ESP32-S3 native USB OTG interface directly via USB-C. **No USB-to-UART bridge chip is used** — the ESP32-S3 USB PHY connects directly.

| Feature | Detail |
|---------|--------|
| USB Standard | USB 2.0 Full Speed (12 Mbps) |
| D+ Pin | GPIO20 |
| D- Pin | GPIO19 |
| Supported Classes | CDC-ACM (Virtual COM), HID, DFU, MSC |
| CC Resistors | 5.1kΩ pull-down on CC1 and CC2 (device mode) |
| ESD Protection | None onboard — add USBLC6-2SC6 for production |
| Firmware Requirement | USB Mode must be set to USB-OTG (TinyUSB) in Arduino IDE |

---

## Quick Start Guide

### Hardware Setup

1. Connect the board to your PC using a **USB-C data cable** (not charge-only)
2. The **PWR LED** should illuminate when powered
3. For UART flashing, connect a USB-UART adapter to **GPIO43 (TX)** and **GPIO44 (RX)**

### Arduino IDE Setup

1. Install the **ESP32 board package** by Espressif (v2.x or later) via Boards Manager
2. Select: **Tools → Board → ESP32S3 Dev Module**
3. Configure the following settings:

| Setting | Value |
|---------|-------|
| USB Mode | USB-OTG (TinyUSB) |
| USB CDC On Boot | Enabled |
| Flash Size | 16MB |
| Partition Scheme | 16M Flash (3MB APP / 9.9MB FATFS) |

4. Select the correct **COM port**

### Entering Download Mode

To flash firmware via USB or UART:

1. Hold the **BOOT** button (GPIO0 LOW)
2. Press and release the **RESET** button
3. Release the **BOOT** button
4. The board is now in download mode — proceed with flashing

### Reading ADS1115 (Example)

Install the **Adafruit ADS1X15** library from Library Manager, then:

```cpp
#include <Adafruit_ADS1X15.h>

Adafruit_ADS1115 ads;

void setup() {
  Wire.begin(8, 9);       // SDA = GPIO8, SCL = GPIO9
  ads.begin(0x48);        // Default ADS1115 address
}

void loop() {
  int16_t v = ads.readADC_SingleEnded(0);  // Read channel AIN0
}
```

---

## Mechanical Specifications

| Parameter | Value |
|-----------|-------|
| Board Form Factor | Custom PCB |
| PCB Layers | 2-layer (recommended 4-layer for production) |
| PCB Thickness | 1.6 mm |
| Connector | USB Type-C Receptacle (16P) |
| GPIO Header Pitch | 2.54 mm (0.1") |
| Operating Temperature | -20°C to +85°C |

<img width="469" height="216" alt="image" src="https://github.com/user-attachments/assets/75934127-8468-4750-953a-d6e844cb395e" />

![Uploading image.png…]()

---

## Safety & Regulatory Notice

> ⚠️ **This board is a development and evaluation module.**

It has **not** been certified under any medical device regulatory framework (FDA, CE, MDR, etc.) and is **NOT approved** for direct clinical use or patient-connected applications without additional safety evaluation, isolation circuitry, and regulatory approval.

- For patient-connected biomedical applications, ensure adequate **galvanic isolation**
- USB-C port shares ground with all GPIO — use **optical or transformer isolation** for ECG/EEG front-ends
- The ADS1115 input range must **not exceed ±VDD** when using external sensors
- **ESD protection** on analog input lines is recommended for production designs

---

## Revision History

| Rev | Date | Author | Changes |
|-----|------|--------|---------|
| 1.0 | April 2026 | — | Initial release |

---

## References

- [Espressif ESP32-S3 Documentation](https://documentation.espressif.com/en/documentList?c=ESP32-S3+SoC+Series&s=ESP32-S3&eol=false)
- [TCA9548A Datasheet — Texas Instruments](https://www.ti.com/product/TCA9548A)
- [ADS1115 Datasheet — Texas Instruments](https://www.ti.com/product/ADS1115)
- [AMS1117 Datasheet](http://www.advanced-monolithic.com/pdf/ds1117.pdf)

---

© 2026 — For Development Use Only
