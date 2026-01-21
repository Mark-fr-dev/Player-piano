# 🎹 Piano Player – Mechatronics Design Project

## 🎯 Project Objective

Design and build a **full-range electromechanical piano-playing system** capable of actuating all **88 keys** of a standard acoustic piano using MIDI performance data.

The project began as a remote collaboration and has since evolved into a long-term development and learning platform focused on **embedded control systems, real-time actuation, and scalable mechatronic design**.

---

## 🧩 System Overview

The piano player is a **modular actuator system** composed of two identical **44-key subsystems**, each responsible for half of the keyboard. The architecture separates:

- **Real-time actuator control** (embedded controller)
- **High-level sequencing and user interaction** (host system)

This separation allows reliable key actuation while supporting flexible external control and observability.

---

## ⚙️ Mechanical Design

The mechanical system was designed for **rapid iteration and modular assembly**:

- Each key is actuated by an independent servo mechanism  
- Structural components are laser-cut plywood and perspex panels  
- Mechanical fasteners are used throughout to allow easy modification and repair  

The modular frame enables partial builds (44 keys at a time), simplifying testing and incremental expansion toward the full 88-key system.

**Design artifacts:**
- [Assembly Photos](http://bit.ly/399H24x)
- [Sketches and Renders](http://bit.ly/3tLXlw5)
- [Mechanical Drawings](http://bit.ly/3vU0D28)

---

## ⚡ Electronics

### Original Architecture (Arduino-Based)

- **88 servo motors**, one per piano key  
- Two microcontroller boards, each controlling **44 keys**  
- Custom interface PCB for:
  - Servo signal routing  
  - Power distribution  
  - MIDI input handling  

Schematics and PCBs were designed in KiCad and fabricated externally.

**PCB documentation:**
- [Controller PCB Design](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Controller_PCB_Design.pdf)
- [Controller PCB Top Layer](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Controller_PCB_top_layer.pdf)
- [Controller PCB Bottom Layer](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Controller_PCB_bottom_layer.pdf)
- [Populated PCB – View 1](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/IMG_8281.jpg)
- [Populated PCB – View 2](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/IMG_8282.jpg)

This architecture successfully demonstrated large-scale servo actuation but revealed limitations in scalability, timing determinism, and power coordination under heavy load.

---

## 💾 Embedded Software

### Initial Implementation

The original firmware:

- Parsed standard MIDI input  
- Mapped MIDI notes to physical actuators  
- Controlled servo position and timing  
- Supported chords, velocity approximation, and diagnostic modes  

While functionally successful, the monolithic structure became increasingly difficult to scale as actuator count and timing complexity increased.

---

## 🔧 System Redesign & Current Architecture

The project is now being **re-architected around a modern embedded control stack** to improve determinism, scalability, and robustness.

### Hardware & Control Evolution

- Migration from direct MCU-driven servo outputs to **I²C-based PWM driver boards**  
- Improved electrical load distribution and power-domain separation  

### Firmware Architecture

- Embedded control migrated to **Zephyr RTOS on STM32**  
- Multi-threaded design separating:
  - MIDI input handling  
  - Event scheduling  
  - Actuator execution  
- Explicit management of timing, task priority, and fault isolation  

This architecture cleanly separates real-time actuation from non-deterministic external inputs.

---

## 🌐 Host Control & Web Interface

A host-side control layer enables:

- MIDI file selection and playback  
- Playlist management  
- Playback control (play, pause, stop, next)  
- Real-time system observation  

A browser-based interface provides remote access and monitoring, acting as a bridge between high-level musical sequencing and low-level actuator control.

![Web UI Screenshot](https://github.com/user-attachments/assets/67916efa-f306-4b98-8527-26a6da34ec72)

---

## 📸 Media

### Arduino Prototype

- [44-Servo Test](https://youtu.be/G8YKHj9sTNw)
- [Playing “The Greatest” – Sia](https://youtu.be/1j-duLm_anE)
- [MacGyver Theme Demo](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Piano_player_Macguyver_2.mp4)

### STM32 / Zephyr Prototype (Current)

**Note:** Only one 44-key subsystem is shown. The system is modular by design, and the demonstrated half represents the finalized control architecture prior to full 88-key reintegration.

- [Calibration Demonstration](https://youtu.be/I6WWZ7fvxso)
- [“River Flows in You” – Yiruma](https://youtu.be/aHvtVR1HKiI)

---

## 🚧 Current Status & Next Steps

### Completed

- Modular mechanical and electrical platform  
- Reliable 44-key real-time actuation  
- MIDI-driven embedded control  
- Host-based playback and monitoring interface  

### In Progress
  
- Porting of MIDI transport mechanisms to a BLE stack
- Debugging and improvements

### Planned Enhancements

- Mechanical redesign to complete the 88 key system
- Per-actuator protection and monitoring  
- Improved articulation and motion profiles  
- Continued optimization for power-aware multi-key actuation  
