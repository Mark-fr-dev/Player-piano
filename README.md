# 🎹 Piano Player – A Mechatronics Design Project

## 🎯 Objective

Develop a full-range electromechanical piano-playing system capable of actuating all 88 keys of a standard acoustic piano.

The project was conducted as a fully remote collaboration with a partner in South Africa, with all mechanical, electronic, and firmware design performed in Taiwan. Initial hardware was fabricated and assembled locally.

The system is currently being redesigned and modernized as a technical R&D platform focused on embedded systems architecture, real-time control, and power-aware firmware design.

---

## 🧩 Design Assumptions and Constraints

Initial design discussions with the partner led to the following explicit constraints and architectural assumptions:
- At the partner’s request, the initial prototype must use an Arduino-class microcontroller, prioritizing accessibility, ease of replacement, and rapid prototyping.
- Hobby servo motors were selected as actuators for the prototype phase, balancing availability, mechanical simplicity, and controllability.
- Alternative actuators evaluated during concept development included solenoids and shape-memory (muscle wire) actuators.
- Musical performance data would be supplied using standard MIDI messages, originating from a PC-class device such as a desktop computer or Raspberry Pi.
- The system must be modular and serviceable, allowing partial builds and incremental expansion during prototyping.
These constraints strongly influenced early decisions around mechanical layout, electronics partitioning, and firmware structure.

---

## ⚙️ Mechanical Design

Mechanical design was carried out in Autodesk Fusion 360, with emphasis on:
- Rapid iteration
- Low-cost fabrication
- Easy disassembly and modification
Due to limited workshop access and budget constraints, the prototype structure was constructed from laser-cut plywood panels, assembled using mechanical fasteners rather than adhesives. This allowed fast design revisions and mechanical experimentation during early testing.

Design artifacts:
- [Piano Player Assembly Pictures](http://bit.ly/399H24x)
- [Piano Player Sketches and Renders](http://bit.ly/3tLXlw5)
- [Piano Player Mechanical Drawings](http://bit.ly/3vU0D28)

---

## ⚡ Electronics

The system requires independent control of **88 independent servo motors**, each corresponding to a single piano key..  
To support this, the design uses **two Arduino Mega boards**, each responsible for 44 keys.

To manage I/O limits and simplify firmware complexity during the initial phase:
- The system was partitioned across two Arduino Mega boards
- Each controller manages 44 keys, corresponding to either the upper or lower half of the keyboard
- A hardware selector switch determines which note range a controller is responsible for

A custom interface shield was designed to handle:
- Servo signal routing
- Power distribution
- External MIDI input
- Board-level interconnects and test access

All schematics and PCB layouts were designed in KiCad, with fabrication performed by JLCPCB.

**PCB documentation:**

- [Controller PCB Design.pdf](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Controller_PCB_Design.pdf)
- [Controller PCB bottom layer.pdf](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Controller_PCB_bottom_layer.pdf)
- [Controller PCB top layer.pdf](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Controller_PCB_top_layer.pdf)
- [Populated PCB – Top View](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/IMG_8281.jpg)
- [Populated PCB – Top View](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/IMG_8282.jpg)

---

## 💾 Embedded Software

TThe original firmware was written in C/C++ and executed on Arduino Mega microcontrollers.

The system receives performance data via a standard 5-pin MIDI IN interface, parses incoming MIDI messages, and translates note and velocity information into actuator motion.

Core firmware responsibilities include:
- Real-time MIDI message parsing and validation
- Mapping MIDI note numbers to physical actuators
- Servo position control and motion profiling
- Approximation of key velocity through timing and displacement
- Coordination and synchronization of multiple actuators during chords
- Diagnostic test modes and basic fault handling

While functionally successful, this monolithic firmware structure exposed several architectural limitations related to timing determinism, scalability, and power coordination as system complexity increased.

---

## 📸 Media
### First prototype (Arduino)
- [Prototype test of 44 servos](https://youtu.be/G8YKHj9sTNw)
- [Old Prototype playing Sia – “The Greatest”](https://youtu.be/1j-duLm_anE)
- [Old Prototype playing the MacGyver theme](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Piano_player_Macguyver_2.mp4)
### New prototype (STM32/Zephyr RTOS)
**Why only 44 servos are shown**
The piano player is designed as a modular system with two identical 44-key subsystems. During the current redesign phase, one subsystem remains under active development, while the second 44-key actuator assembly was supplied to a project partner as a standalone evaluation and testing platform.

As a result, the videos demonstrate only one half of the keyboard, but accurately represent the control architecture being validated prior to full 88-key reintegration.
- [Zephyr Prototype - Calibration](https://youtu.be/I6WWZ7fvxso?si=0EKCNAyHqwldLYc-)
- [Zephyr prototype - River Flows in You - Yaruma](https://youtu.be/aHvtVR1HKiI?si=-SzJifrcjJAI7ls0)
  
---

## 🔧 System Redesign & Ongoing R&D (Zephyr RTOS Learning Project)

To address limitations discovered during the Arduino-based implementation, the project is currently being re-architected as a modern embedded systems platform.

This redesign serves both as a functional upgrade and as a hands-on learning project focused on Zephyr RTOS, ARM Cortex-M systems, and real-time firmware design.

### ⚡**Hardware & Electronics Evolution**
- Migration from direct MCU-driven servo outputs to I2C-based PWM driver boards (e.g. PCA9685-class devices)
    - Improves scalability beyond MCU timer limits
    - Reduces ISR and timing jitter
    - Enables cleaner electrical load distribution across multiple power domains
- Redesign of power and control electronics to better support simultaneous multi-key actuation
- Mechanical refinements to improve structural stability during dense chord play and rapid passages

### 🧠 **Firmware & System Architecture**
- Porting legacy Arduino firmware to Zephyr RTOS running on an STM32WB55 (ARM Cortex-M4/M0+)
- Introduction of a multi-threaded architecture, separating:
    - MIDI input parsing
    - Event queuing and scheduling
    - Servo motion execution
- Explicit handling of timing determinism, task prioritization, and inter-thread communication
- Improved fault isolation and system robustness through RTOS primitives
### 🔌**Interfaces & Control**
- Evaluation of USB-MIDI and Bluetooth-MIDI as alternatives to traditional DIN MIDI
- Integration with a Raspberry Pi for higher-level control, song selection, and testing workflows
### 🛠️ **Future Enhancements**
- Introduction of per-actuator temperature monitoring and protection, previously considered in early hardware designs
- Refinement of servo motion algorithms for smoother articulation and more musical phrasing
- Continued exploration of power-aware scheduling and coordinated actuator behavior under load
