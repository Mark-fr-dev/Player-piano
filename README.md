# 🎹 Piano Player – Mechatronics Design Project

## 🎯 Project Objective

Design and build a **full‑range electromechanical piano‑playing system** capable of actuating all **88 keys** of a standard acoustic piano using MIDI performance data.

The project began as a **fully remote collaboration**, with mechanical, electrical, and firmware development performed in Taiwan. Over time, it evolved into a long‑term **embedded systems and real‑time control R&D platform**, serving as a vehicle for learning, architectural experimentation, and scalable mechatronic design.

---

# 🧩 System Overview

The system is built around a **modular actuator architecture** consisting of two identical **44‑key subsystems**, each responsible for half of the keyboard. This modularity supports:

- Incremental mechanical assembly  
- Independent subsystem testing  
- Easier debugging and refinement  
- Remote collaboration and partial builds  

The architecture cleanly separates:

- **Real‑time actuator control** (embedded controller)  
- **High‑level sequencing, playback, and user interaction** (host system)  

This separation ensures direct actuation while enabling flexible external control, observability, and experimentation.

---

# 🧩 Early Design Constraints & Rationale

Initial design discussions established several constraints that shaped the first prototype:

- The system needed to use an **Arduino‑class microcontroller** for accessibility and ease of replacement and institutional knowledge at the partner's request.  
- **Hobby servos** were selected for availability, simplicity, and low‑cost experimentation.  
- The system had to be **modular and serviceable**, supporting partial builds and rapid iteration.  
- Musical performance data would be supplied via **standard MIDI**, originating from a PC or Raspberry Pi.  
- The mechanical structure needed to be **low‑cost**, easy to fabricate, and modifiable without specialized tools.

These constraints strongly influenced the early mechanical layout, electronics partitioning, and firmware structure.

---

# ⚙️ Mechanical Design

Mechanical design was carried out in **Autodesk Fusion 360**, with emphasis on:

- Rapid iteration  
- Low‑cost fabrication  
- Easy disassembly and modification  

Due to limited workshop access and budget constraints, the prototype frame was built from **laser‑cut plywood and perspex**, assembled using mechanical fasteners rather than adhesives. This allowed fast design revisions and mechanical experimentation during early testing.

**Design artifacts:**

- [Assembly Photos](http://bit.ly/399H24x)  
- [Sketches and Renders](http://bit.ly/3tLXlw5)  
- [Mechanical Drawings](http://bit.ly/3vU0D28)

---

# ⚡ Electronics

## Original Architecture (Arduino‑Based)

The first-generation system required independent control of **88 servo motors**, one per piano key. To support this:

- Two **Arduino Mega** boards were used, each controlling **44 keys**  
- A hardware selector switch assigned each board to its note range  
- A custom interface PCB handled:  
  - Servo signal routing  
  - Power distribution  
  - External MIDI input  
  - Board‑to‑board interconnects and test access  

All schematics and PCB layouts were created in **KiCad**, with fabrication performed by JLCPCB.

**PCB documentation:**

- [Controller PCB Design](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Controller_PCB_Design.pdf)  
- [Controller PCB Top Layer](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Controller_PCB_top_layer.pdf)  
- [Controller PCB Bottom Layer](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Controller_PCB_bottom_layer.pdf)  
- [Populated PCB – View 1](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/IMG_8281.jpg)  
- [Populated PCB – View 2](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/IMG_8282.jpg)

While functional, this architecture revealed limitations:

- Servo timing jitter due to MCU timer constraints  
- Power distribution challenges during dense chord play  
- Increasing firmware complexity as features grew  
- Difficulty scaling beyond the monolithic control loop  

These limitations motivated a full system redesign.

---

# 💾 Embedded Software

## Initial Firmware (Arduino)

The original firmware:

- Parsed standard MIDI input  
- Mapped MIDI notes to actuators  
- Controlled servo position and motion timing  
- Coordinated multi‑key actuation for chords  
- Included diagnostic and test modes  

Although successful, the monolithic structure struggled with timing problems as the Arduino was severely underpowered for the job.

---

# 🔧 System Redesign & Modern Architecture

The system is now being re‑architected as a **modern embedded control platform** focused on determinism, scalability, and robustness.

## Hardware & Control Evolution

- Migration from direct MCU‑driven servo outputs to **I²C‑based PWM driver boards (PCA9685‑class)**  
- Improved electrical load distribution and power‑domain separation  
- Cleaner signal routing and reduced ISR load  

## Firmware Architecture (Zephyr RTOS)

The embedded control layer has been ported to **Zephyr RTOS running on an STM32WB55**, introducing:

- A multi‑threaded architecture separating:  
  - MIDI input parsing  
  - Event scheduling  
  - Actuator motion execution  
- Explicit timing determinism and task prioritization  
- Robust inter‑thread communication  
- Improved fault isolation and system resilience  

This architecture cleanly decouples real‑time actuation from non‑deterministic external inputs.

---

# 🌐 Host Control & Web Interface

To support remote testing and flexible playback, a host‑side control layer provides:

- MIDI file browsing and selection  
- Playlist management  
- Playback control (play, pause, stop, next)  
- Real‑time system observation  

### Evolution of the Host System

The webserver originally ran on a **Raspberry Pi 2B**, but under heavy load (multiple clients, large MIDI files, real‑time updates) the Pi became CPU‑bound. To improve responsiveness and reliability, the system was migrated to a **more powerful openSUSE Linux workstation**, resulting in:

- Faster page loads  
- Lower latency in control commands  
- More stable long‑duration playback sessions  
- Headroom for future features (e.g., video streaming, analytics)  

This migration reflects the system’s evolution from a simple test interface to a robust orchestration layer.

![Web UI Screenshot](https://github.com/user-attachments/assets/67916efa-f306-4b98-8527-26a6da34ec72)

---

# 📸 Media

## Arduino Prototype

- [44‑Servo Test](https://youtu.be/G8YKHj9sTNw)  
- [Playing “The Greatest” – Sia](https://youtu.be/1j-duLm_anE)  
- [MacGyver Theme Demo](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Piano_player_Macguyver_2.mp4)

## STM32 / Zephyr Prototype (Current)

**Note:** Only one 44‑key subsystem is shown. The second subsystem was provided to a collaborator for independent testing, and the architecture is validated on a half‑keyboard before full 88‑key reintegration.

- [Calibration Demonstration](https://youtu.be/I6WWZ7fvxso)  
- [“River Flows in You” – Yiruma](https://youtu.be/aHvtVR1HKiI)

---

# 🚧 Current Status & Next Steps

## Completed

- Modular mechanical and electrical platform  
- Reliable 44‑key real‑time actuation  
- MIDI‑driven embedded control  
- Host‑based playback and monitoring interface  
- Migration of host system from RPi2B to openSUSE workstation  

## In Progress

- BLE‑MIDI transport integration  
- Debugging and performance improvements  
- Refinement of actuator motion profiles  

## Planned Enhancements

- Mechanical completion of the full 88‑key system  
- Per‑actuator temperature monitoring and protection  
- Power‑aware scheduling for dense chord passages  
- Further optimization of real‑time control algorithms  

---
