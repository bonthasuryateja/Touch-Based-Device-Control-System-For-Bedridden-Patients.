# Touch-Based Device Control System for Bedridden Patients

An embedded systems project built on the **LPC2148 (ARM7TDMI-S)** microcontroller that lets bedridden patients control household devices — a fan, a light, and a buzzer for emergency alerts — through a resistive touch screen module, with a password-protected access system for security.

## Overview

The system is designed to give bedridden or physically limited patients an easy way to operate essential devices around them without needing to physically reach for switches. A touch screen module (connected via UART) is divided into virtual button regions; touching a region toggles the corresponding device. All access is protected by a 4-digit password stored securely in an external SPI EEPROM, entered via a 4x4 matrix keypad and displayed on a 16x2 LCD.

## Block Diagram

```
                     ┌─────────────┐
  4x4 MATRIX  ─────► │             │ ─────► LCD
  KEYPAD             │             │
                      │             │ ─────► DEVICE-1 (LED1 / Fan)
  I.SW      ──[EINT]► │   LPC2148   │
                      │             │ ─────► DEVICE-2 (LED2 / Light)
                      │             │ ◄────► SPI ◄──► AT25LC512 EEPROM
  TOUCH SCREEN───[UART0]►           │
  MODULE               │             │ ─────► BUZZER
                      └─────────────┘
```
> **Note:** Apply 3.3V as the operating voltage to the SPI-based EEPROM (AT25LC512).

## Features

- **Password-protected access** — 4-digit password entry via keypad, verified against a value stored in external EEPROM before granting control.
- **Password change facility** — An external interrupt (EINT1), triggered by a switch, lets the user change the current password after verifying the existing one.
- **Touch-based device control** — Touch screen coordinates (X, Y, Z) are parsed from a UART data stream and mapped to specific screen regions:
  - Touch-enable/disable toggle
  - Buzzer ON/OFF
  - Fan (Device-1) ON/OFF
  - Light (Device-2) ON/OFF
- **Real-time status display** — LCD shows live ON/OFF status of the touch-enable, buzzer, fan, and light.
- **Interrupt-driven UART reception** — Touch screen data is received via UART0 RX interrupt for reliable, non-blocking communication.
- **Non-volatile password storage** — Password is stored and retrieved from an AT25LC512 SPI EEPROM so it persists across power cycles.

## Hardware

| Component | Interface | Purpose |
|---|---|---|
| LPC2148 (ARM7TDMI-S) | — | Main microcontroller |
| 4x4 Matrix Keypad | GPIO (Port 1) | Password entry |
| 16x2 LCD | GPIO (8-bit mode) | Status and prompt display |
| Touch Screen Module | UART0 | Receives touch (X, Y, Z) coordinates |
| AT25LC512 SPI EEPROM | SPI | Persistent password storage |
| Push Switch | EINT1 | Triggers password change routine |
| Buzzer | GPIO | Emergency/audible alert |
| Device-1 (LED/Fan) | GPIO | Simulated fan control |
| Device-2 (LED/Light) | GPIO | Simulated light control |

## Repository Structure

```
├── majormain.c          # Main application logic, password flow, touch region handling, EINT1 ISR
├── UART_INT.c/.h        # UART0 init and interrupt-driven receive of touch screen data
├── SPI.c/.h              # SPI driver + AT25LC512 EEPROM read/write + password I/O via keypad
├── spi_defines.h        # SPI pin/clock/command definitions
├── KPM.c/.h              # 4x4 matrix keypad scanning and key lookup
├── KPM_defines.h         # Keypad row/column pin definitions
├── lcd.c/.h              # LCD driver (init, char/string/number display)
├── lcd_defines.h         # LCD command and pin definitions
├── cfgportpinfunc.c/.h  # Generic pin function/mux configuration utility
├── delay.c/.h            # Microsecond/millisecond/second software delays
├── defines.h             # Bit manipulation macros (SETBIT, CLRBIT, WRITEBYTE, etc.)
├── types.h               # Typedefs for fixed-width types (u8, u16, u32, s8, s32, f32, f64)
└── README.md
```

## How It Works

1. **Startup** — The system initializes the LCD, keypad, UART0, and SPI, then reads the stored password from the EEPROM.
2. **Authentication** — The user enters a 4-digit password via the keypad. Digits are masked (`*`) on the LCD, and a `+` key acts as backspace. Entry only proceeds once the correct password is entered.
3. **Main control loop** — Once unlocked, the system waits for touch screen input over UART0. The interrupt handler buffers incoming characters into a string until a carriage return, then sets a flag indicating new coordinate data is ready.
4. **Coordinate parsing** — The main loop extracts X, Y, Z values from the buffered string and checks which predefined screen region was touched, toggling the corresponding device.
5. **Password change** — Pressing the external switch (EINT1) interrupts the main flow, asks for the current password, and if verified, lets the user set and confirm a new one, which is written back to the EEPROM.
6. **Status feedback** — The LCD continuously reflects the current ON/OFF state of touch-enable, buzzer, fan, and light.

## Getting Started

1. Open the project in **Keil µVision** (or your preferred ARM7/LPC2148 toolchain).
2. Ensure `LPC21xx.H` is available in your compiler's device header path.
3. Build and flash to an LPC2148 board using a JTAG/ISP programmer (e.g., Flash Magic).
4. Wire up the hardware per the block diagram above. **Apply 3.3V to the AT25LC512 EEPROM.**
5. On first run, uncomment the initial `Spi_eeprom_pagewrite()` call in `majormain.c` to write a default password into the EEPROM, flash and run once, then re-comment it for normal operation.

## Output  
<img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/020ab5a8-c5ba-4323-9232-b242fc56e9a6" />  <img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/25ab1418-5b16-4cdd-b618-62f0fa13c9c3" />  <img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/b0250375-40cd-45dd-bdcc-b1e37a1a0fd6" />  <img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/e406ece5-72ef-4922-87ba-002bbb427d11" />  <img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/ec5f54a2-287b-4345-88f1-a252052a14b7" />  <img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/2bf0ba82-7032-4ac1-a43c-ac1d8a0beeea" />  





