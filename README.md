# 🎹 Piano Player – A Mechatronics Design Project

## 🎯 Objective

A client affiliated with a well-known South African artist commissioned the development of an electromechanical system capable of playing all 88 notes of a piano. Because this was a remote collaboration, all design work had to be produced in Taiwan.

This project is currently **on hold**, pending further client direction.

---

## 🧩 Design Assumptions and Constraints

Following initial discussions with the client, the key design constraints and assumptions were:

- At the client’s request, the system must use an **Arduino-class microcontroller**.
- Actuators for the prototype will use **hobby servo motors** to simplify sourcing, replacement, and prototyping. (Alternative actuators considered: solenoids, muscle wire/shape-memory wire.)
- The firmware will receive input via **standard MIDI commands**, provided by a PC-like device (desktop PC or Raspberry Pi).

---

## ⚙️ Mechanical Design

Mechanical design was performed in **Autodesk Fusion 360**.  
Due to limited fabrication budget and workshop access, the prototype was built from **laser-cut plywood** panels, assembled with screws for rapid iteration and ease of modification.

- [Piano Player Assembly Pictures](http://bit.ly/399H24x)
- [Piano Player Sketches and Renders](http://bit.ly/3tLXlw5)
- [Piano Player Mechanical Drawings](http://bit.ly/3vU0D28)

---

## ⚡ Electronics

The system requires control of **88 independent servo motors**.  
To support this, the design uses **two Arduino Mega boards**, each responsible for 44 keys.

A custom interface shield was created to route signals, power distribution, and control to each group of servos.

All schematics and PCB designs were developed in **KiCad**, and the boards were fabricated by **[JLCPCB](https://jlcpcb.com/)**.

**PCB files and images:**

- [Controller PCB Design.pdf](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Controller_PCB_Design.pdf)
- [Controller PCB bottom layer.pdf](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Controller_PCB_bottom_layer.pdf)
- [Controller PCB top layer.pdf](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Controller_PCB_top_layer.pdf)
- [Populated PCB – Top View](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/IMG_8281.jpg)
- [Populated PCB – Top View](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/IMG_8282.jpg)

---

## 💾 Embedded Software

The firmware receives piano performance data via a **standard MIDI IN connector**.  
A hardware selector switch determines whether the controller handles the upper or lower 44 notes of the keyboard.

Firmware responsibilities include:

- MIDI message parsing  
- Servo positioning control  
- Key timing and velocity approximation  
- Actuator synchronization  
- Basic error handling and test modes  

---

## 📸 Media

- [Prototype test of 44 servos](https://youtu.be/G8YKHj9sTNw)
- [Current Prototype playing Sia – “The Greatest”](https://youtu.be/1j-duLm_anE)
- [Current Prototype playing the MacGyver theme](https://github.com/Mark-fr-dev/Player-piano/blob/main/files/Piano_player_Macguyver_2.mp4)

---

## 🔧 Planned Improvements / Future Work

With the migration to an STM32WB55 microcontroller and Zephyr RTOS, several earlier design limitations will be addressed with a new architecture:

### **Hardware & Electronics Updates**
- Transition from direct servo control to **I2C-based PWM driver boards** to improve scalability, timing accuracy, and electrical load distribution.
- Redesign of actuator control electronics to better support synchronized multi-key actions.
- Improved mechanical anchoring to prevent lifting/shifting during dense chord play.

### **Firmware & System Architecture**
- Porting the original Arduino firmware to **Zephyr RTOS**, enabling structured multitasking (MIDI parsing, scheduling, actuator control).
- Evaluate **USB-MIDI or Bluetooth-MIDI** input for more flexible song control.

### **Software Enhancements**
- Reintroduce actuator temperature monitoring and protection once the new hardware stack is finalized.
- Improve servo timing algorithms for smoother, more musical playability.
