# Custom STM32 IMU Sensor Node

This repository contains the complete hardware design files for an ultra-compact, high-precision **STM32 Inertial Measurement Unit (IMU) Sensor Board**. Hierarchically engineered from the chip-down level in **Altium Designer**, this board serves as a standalone tracking node capable of real-time orientation, acceleration, and motion telemetry processing.

<img width="720" height="591" alt="image" src="https://github.com/user-attachments/assets/82c7a113-6f6f-4182-b981-1a819208aea3" />


---

## 🚀 Key Hardware Features

* **Chip-Down STM32 Architecture:** Custom core design utilizing an STM32 MCU in a space-saving QFP package with an optimized external crystal routing layout.
* **Onboard IMU Integration:** High-sensitivity Inertial Measurement Unit co-located with a dedicated decoupling capacitor matrix for noise-free inertial sensing.
* **Filtered USB Power Stage:** Onboard voltage regulation equipped with passive EMI filtering to protect sensitive analog sensor rails.
* **Advanced Debug Interfacing:** Full 4-wire Serial Wire Debug (SWD) breakout supporting real-time bare-metal flashing, hardware tracing, and live diagnostics.
* **Ultra-Compact Form Factor:** Miniature square PCB profile with robust mounting holes, customized with clear GPIO silkscreen legends and signature "M.ASHOUR" branding.

---

## 🔌 Detailed Circuit Architecture & Subsystems

The schematic architecture is executed using strict hierarchical top-down design rules, isolating functional blocks into specialized sub-sheets:

<img width="802" height="413" alt="image" src="https://github.com/user-attachments/assets/c5437182-0079-43f8-916f-041f57e39048" />


### 1. Central MCU Core Subsystem (`U_MCU`)
* **Brain:** Hosts the **STM32** micro-controller. The routing ensures minimal trace lengths for high-speed digital lines and incorporates a distributed decoupling matrix (`C9, C11, C13, C14`) to stabilize core operating voltages.

### 2. Inertial Measurement Unit Subsystem (`U_IMU`)
* **Sensor Core:** Integrates an advanced **IMU IC (`U1`)** tasked with motion tracking.
* **Bus Communication:** Communicates with the main STM32 via a hardware **I2C Serial Bus** (`IMU_SCL` and `IMU_SDA`) equipped with optimized pull-up resistor networks.
* **Event Interfacing:** Features a dedicated hardware interrupt line (`IMU_INT`) routed straight to an MCU GPIO pin to support low-power sleep wake-ups upon sudden shock or motion detection.

### 3. Power Management & USB Filtering (`U_USB_Power`)
* **Power Entry:** Draws stable +5V DC voltage via a robust Micro-USB connector (`J1`).
* **EMI Suppression:** Integrates a **Ferrite Bead (FB1)** coupled with decoupling capacitors (`C16, C17`) directly on the input line to filter out high-frequency switching noise originating from external USB supplies.
* **Voltage Regulation:** The filtered supply feeds a low-dropout linear regulator (**AMS1117-3.3 (`U3`)**) to output a rock-solid **3V3** power rail for the digital core, verified visually by a status LED (`D1`).

### 4. Clock Oscillation Subsystem (`U_XTAL`)
* **HSE Oscillator:** Drives the main MCU subsystem using an external crystal oscillator component (`X1`).
* **Impedance Matching:** Tuned alongside dual load capacitors (`C1, C2`) connected to the `PH0_IN` and `PH1_OUT` pins to maintain a stable, jitter-free master clock frequency.

### 5. Debugging & I/O Expansion (`U_debug`)
* **SWD Interface:** Fully exposes the Serial Wire Debug array through a secure header (`J2`). Includes data (`MCU_SWDIO`), clock (`MCU_SWDCLK`), trace output (`MCU_SWO`), and hardware reset (`MCU_NRST`) lines.
* **GPIO Breakout:** Routes spare computational pins to a low-profile terminal connector (`J3`) labeled clearly for modular sensor expansions.
