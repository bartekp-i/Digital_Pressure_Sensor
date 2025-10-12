
# Digital Pressure Sensor Project - Technical Documentation

## Introduction  

This repository will host the hardware, firmware, and software for a high-precision **Digital Pressure Sensor** designed for laboratory, industrial, and hobbyist applications. The system combines analog signal conditioning, 16-bit resolution analog-to-digital conversion (ADC), and a Raspberry Pi RP2040 microcontroller to deliver accurate pressure measurements in real time. The device supports dual power inputs (USB-C and DC jack), configurable measurement ranges (2-bar and 10-bar modes), and a user-friendly OLED interface for data visualization.
---

## Components and Functions

### 1. **Power Input and Protection**
#### **USB-C Connector (USB4105-GF-A-060)**
- **Function**: Primary power input and data communication interface.
- **Circuit Details**:
  - **Filtering Circuit**: Includes a ferrite bead (EMI suppression) and capacitors (decoupling and noise filtering).
  - **ESD Protection**: PRTR5V0U2X,215 diode for transient voltage suppression on USB data lines.
  - **Resistors**: 
    - 27Ω resistors on D+/D- lines for impedance matching.
    - 5.1kΩ resistors on CC1/CC2 lines for USB-C configuration.
  - **LED**: Indicates USB connection status.
  - **Schottky Diode**: Reverse polarity protection.

#### **DC Power Connector (694108301002 - Wurth Electronics)**
- **Function**: Secondary DC power input (e.g., for standalone operation).
- **Circuit Details**:
  - **Filtering Circuit**: Inductor (choke) and capacitors for ripple/noise suppression.
  - **LED**: Indicates DC power presence.
  - **Schottky Diode**: Reverse polarity protection.

---

### 2. **Core Processing and Memory**
#### **RP2040 Microcontroller**
- **Function**: Dual-core ARM Cortex-M0+ processor for data acquisition, processing, and system control.
- **Design Notes**:
  - **Decoupling Capacitors**: Placed near power pins to stabilize voltage and filter high-frequency noise.
  - **SWD Debug Interface**: Header for programming/debugging via Serial Wire Debug (SWD).

#### **W25Q16JVUXIQ QSPI Flash**
- **Function**: Stores firmware, calibration data, and user settings.
- **Boot Button**: Tactile switch to force the RP2040 into bootloader mode for firmware updates.

#### **ABM8-272-T3 Crystal Oscillator**
- **Function**: Provides a stable 12MHz clock signal for the RP2040’s real-time clock or low-power timing.

---

### 3. **Power Management**
#### **SPX5205M5-L-3-3/TR LDO Regulator**
- **Function**: Converts input voltage (5V from USB/DC) to 3.3V for the RP2040 and peripherals.
- **Specifications**: 150mA output current, low dropout voltage.

#### **ADR3440ARJZ-R7 Voltage Reference**
- **Function**: Provides a stable 4.0V reference for the ADC (AD7680ARMZ).
- **Accuracy**: Ultra-low drift (±5ppm/°C) for high-precision measurements.

---

### 4. **Signal Conditioning and ADC**
#### **AD8606ARMZ Op-Amp (Buffer)**
- **Function**: Buffers the sensor signal and scales down the 5V sensor output to 0.5V using a voltage divider.
- **Configuration**: Unity-gain buffer to isolate the sensor from downstream circuits.

#### **AD8605ARTZ Op-Amp (Subtractor)**
- **Function**: Subtracts 0.5V offset from the sensor’s 0.5–4.5V output, producing a 0–4V signal.
- **Output Scaling**: A voltage divider further scales the 0–4V signal to 0–3.3V for the ADC’s input range.

#### **AD7680ARMZ ADC**
- **Function**: Converts the conditioned analog signal to a 16-bit digital value.
- **Interface**: SPI communication with the RP2040.
- **Dynamic Range**: 0–3.3V input, corresponding to 0–10 bar pressure (configurable).

---

### 5. **User Interface**
#### **1.5-inch OLED RGB Display**
- **Function**: Displays real-time pressure data, units (bar/psi), and operational mode (2 or 10 bar range).
- **Connection**: Header/THT pads for I2C or SPI interface.

#### **Tactile Switches**
- **Mode Button**: Toggles between 2-bar and 10-bar measurement ranges.
- **Unit Button**: Switches pressure units (e.g., bar, psi, kPa).

---

## Mechanical Considerations

### **PCB Design**
- **Dimensions**: 72mm × 40mm (rectangular form factor).
- **Layers**: 2-layer design for cost efficiency.
- **Mounting Features**:
  - **4× Edge Mounting Holes**: M2.5 screws for securing the PCB to enclosures or surfaces.
  - **4× OLED Mounting Points**: Aligns with the display module’s screw terminals.
- **Routing**:
  - Analog and digital grounds separated to minimize noise coupling.
  - Short traces for critical signals (ADC, op-amp outputs).

## Board Functionality

### **Power Flow**
1. **Input Selection**: USB-C or DC jack (5V input prioritized by Schottky diodes).
2. **Voltage Regulation**: SPX5205 LDO converts 5V to 3.3V for the RP2040, Flash, and peripherals.
3. **Reference Stability**: ADR3440 provides a stable 4.0V reference to the ADC.

### **Signal Path**
1. **Sensor Input**: Pressure sensor connects via via pads, outputting 0.5–4.5V proportional to pressure.
2. **Buffering and Scaling**: AD8606 buffers and scales the signal to 0.5V, followed by AD8605 subtracting 0.5V to remove offset.
3. **ADC Conversion**: AD7680 digitizes the 0–3.3V signal, transmitting data to the RP2040 via SPI.

### **Data Processing**
- The RP2040:
  - Samples ADC data.
  - Applies calibration coefficients stored in QSPI Flash.
  - Drives the OLED to display pressure, units, and mode.
  - Supports USB CDC (serial communication) for data logging.

### **User Interaction**
- **Mode Switching**: Tactile buttons reconfigure the ADC’s input range and display units.
- **Boot Mode**: Holding the boot button during reset enables firmware updates via USB mass storage.


### **GUI**
- **Software**: MicroPython-based GUI for real-time pressure visualization and data export.
- **Features**:
  - Toggle between units and modes.
  - Save calibration profiles to Flash.

---

## PCB layout and 3D model
<img width="1176" height="828" alt="image" src="https://github.com/user-attachments/assets/53613373-b655-4e0b-9a25-b60e087893ed" />

![image](https://github.com/user-attachments/assets/0f9087a9-8f40-4cdb-97b1-04e057f342b1)

![image](https://github.com/user-attachments/assets/36c5bb07-4236-40aa-aaa2-840e889b1bdb)

![image](https://github.com/user-attachments/assets/dc390f30-73cc-4f04-8489-854485bd8bbc)

![image](https://github.com/user-attachments/assets/67a986f7-ab56-4080-bcab-ebede8fd55e3)

