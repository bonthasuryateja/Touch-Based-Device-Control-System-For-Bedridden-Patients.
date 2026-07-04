# Touch-Based Device Control System for Bedridden Patients

An assistive, secure medical device control framework engineered for the **ARM7 LPC2148 micro-controller**. The system provides individuals with limited mobility a low-effort interface to operate essential appliances using a resistive touch panel. Unauthorized or accidental triggers are prevented by a password-authentication barrier backed by non-volatile storage.

## 🚀 Key Features
* **Resistive Touch Interface:** Low-force coordinate mapping via a UART-interrupt-driven touchscreen to trigger appliance relays.
* **EEPROM Guarded Security:** Uses an **AT25LC512** SPI EEPROM operating at 3.3V for reliable, non-volatile password storage and dynamic modification runtime tracking.
* **Interrupt-Driven Comms:** Handled via low-overhead UART1 RX ISR routines to sample real-time coordinate data stream packets instantly.
* **Fail-Safe Alerts:** Active piezo buzzer handling for sensory feedback, password failure warnings, and touch-activated emergency alarms.

---

## 🛠️ System Architecture

### Hardware Components
* **MCU:** NXP LPC2148 (ARM7TDMI-S)
* **Storage:** AT25LC512 (512 Kb SPI EEPROM)
* **Inputs:** 4-Wire Resistive Touchscreen Module, 4x4 Matrix Keypad
* **Outputs:** 16x2 Character LCD, LEDs (Device Simulators), 5V Active Buzzer

### Software Stack
* **Language/Toolchain:** Embedded C / ARM Compiler v5
* **IDE:** Keil uVision 5
* **Utility:** Flash Magic (In-System Programming via UART0)

---

## 📂 Modular Implementation Checklist

Development follows a strict hardware validation pipeline before final system integration:

1. **LCD Driver Verification:** Print character, string, and integer constants.
2. **Keypad Capture:** Read matrix inputs and flash hex values onto the LCD.
3. **SPI EEPROM Handling:** Complete sequential atomic `BYTE WRITE` and `BYTE READ` test operations over SPI0.
4. **Touch Module Comms:** Intercept raw coordinate bytes via PC debugging using a MAX232 transceiver circuit, followed by native UART1 interrupt parser development.
5. **Main System Integration:** Combine peripheral states into a single unified security-critical application loop (`projectmain.c`).

---

## 🔧 Operational Logic Flow  <img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/a40d151d-6a84-4440-95c8-fe9fd70f298b" />  <img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/8ce5e442-aca7-4c25-ae57-0fa5da82f64c" />  <img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/2ed6e76f-8b2d-484b-95d7-c679dba334a2" />
<img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/92081a9a-73cd-417e-81b3-a143704883ef" />  
<img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/684d13c9-316f-4303-a941-01c4f1b81066" />  
<img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/c6c10d30-271c-484c-a421-a40a50c6002d" />  
<img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/4cb1ba6c-c963-4c41-b9cb-8039ed6304fe" />  
<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/d4d2c109-3b24-4a84-b0d2-4e6630e300f3" />  


