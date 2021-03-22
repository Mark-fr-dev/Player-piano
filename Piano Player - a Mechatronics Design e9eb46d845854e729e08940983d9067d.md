# Piano Player - a Mechatronics Design

## 🎯Objective

The client, who is affiliated with a well known South African artist, required the development of a system capable of playing all the notes of a piano. As this was a remote working relationship, all designs would later be implemented in South Africa by a local engineer or technician once the design is completed.

This project is currently on hold

## ⁉️General Design Assumptions

After discussions with the client with regards the final implementation, the following design assumptions and constraints were agreed apon:

- At the customer's request, the system will use an Arduino based micro controller
- The actuators for the prototype will be simple hobby servo motors for simplicity and replaceability. Other options considered were: solenoids, muscle wires
- The software will use standard MIDI commands to receive music from a PC-like device, this could be a real PC or a raspberry pi.

## 🦾Mechanical Design

All mechanical design was performed using Autodesk Fusion 360. Having limited budget and access to facilities, it was decided to make the prototype with laser-cut plywood flat panels assembled with screws.

[Piano Player Assembly Pictures](Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/Piano%20Player%20Assembly%20Pictures%20b2c6857be631407b80337fd8818d164d.csv)

[Piano Player Sketches and Renders](Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/Piano%20Player%20Sketches%20and%20Renders%209399ab6f0d6e467e9a09f3d7f2965097.csv)

[Piano Player Mechanical Drawings](Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/Piano%20Player%20Mechanical%20Drawings%204d276254374442d387b28b677f1f27b1.md)

## ⚡Electronics

The project requires the control of 88 servo motors, one way to achieve this is with 2 Arduino Mega boards. An Arduino "shield" was designed to interface with the 44 servos on each channel.

Electronic schematic and PCB design was performed using KiCad.

PCBs were fabricated by [JLC PCB](https://jlcpcb.com/).

[Controller PCB Design.pdf](Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/Controller_PCB_Design.pdf)

[Controller PCB bottom layer.pdf](Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/Controller_PCB_bottom_layer.pdf)

[Controller PCB top layer.pdf](Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/Controller_PCB_top_layer.pdf)

![Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/IMG_8281.jpg](Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/IMG_8281.jpg)

![Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/IMG_8282.jpg](Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/IMG_8282.jpg)

## 💾Embedded software

The software accepts a piano song via MIDI connector in the standard MIDI configuration. A hard switch sets whether the controller is controlling the top or bottom 44 notes.

## 📸 Media

[Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/Piano_player_wave.mp4](Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/Piano_player_wave.mp4)

An initial prototype test of 44 servos

[Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/Piano_player_sia_3.mp4](Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/Piano_player_sia_3.mp4)

Current Prototype playing Sia -The Greatest

[Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/Piano_player_Macguyver_2.mp4](Piano%20Player%20-%20a%20Mechatronics%20Design%20e9eb46d845854e729e08940983d9067d/Piano_player_Macguyver_2.mp4)

Current Prototype playing the MacGuyver theme song

## ✅ Still To Do

- Improvements

    There are improvements to be made to the software and the mechanical hardware system. At present there are also a few errors on the PCB that could be fixed in a future revision of the board.

    Because of the use of a desktop rubber piano which does not give way as the keys are pressed, the player also has a tendency to lift itself up and move off the correct keys when too many keys are played concurrently. This problem is being addressed.

- Still to implement

    The PCB was designed to monitor the temperatures of each actuator to prompt maintenance before failure. This still needs to be implemented and tested in both hardware and software.

- Raspberry Pi

    At present the MIDI files are sent by a desktop PC, but future development should have them sent by a WIFI enabled Raspberry Pi, accessible from the internet or mobile phone to upload new files or control the songs that are being played.