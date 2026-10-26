V2V BeamComm — Amethyst
Open-source starter repository for a light-based vehicle-to-vehicle emergency-warning prototype.
> **Prototype status:** This repository contains illustrative Arduino firmware and documentation for a controlled bench demonstration. It is not automotive-grade software and must not be used as a real vehicle safety system.
Overview
V2V BeamComm explores sending short emergency messages from one compatible unit to another using a modulated light source and an optical receiver. The receiver validates the message and shows it on a 16×2 LCD with an audio alert.
Repository contents
`firmware/transmitter/transmitter.ino` — sends a short coded message using an LED output.
`firmware/receiver/receiver.ino` — detects pulse timing from a digital optical receiver, validates the message and triggers LCD/buzzer alerts.
`docs/TECHNICAL_DESIGN.md` — architecture, interfaces, assumptions and limitations.
`docs/architecture.mmd` — Mermaid architecture diagram.
`data/sample_messages.csv` — sample message catalogue for demonstrations.
`tests/TEST_PLAN.md` — bench test cases and acceptance criteria.
`LICENSE` — MIT License.
Hardware assumptions
2 × Arduino Uno (or compatible 5 V board)
LED/light transmitter driven through a suitable transistor/driver stage; do not drive a high-power LED directly from an Arduino pin
Digital-output optical receiver module compatible with the receiver firmware
16×2 LCD with I²C backpack (address commonly `0x27`; adjust if needed)
Active buzzer module
Breadboard, resistors, wires and USB cables
The sketches use a simple pulse-width encoding: each message character is sent as 8 bits, most-significant bit first. A long start pulse begins a frame; a short inter-bit gap separates bits. This is a learning prototype, not a standardized LiFi/VLC protocol.
Setup
Install the Arduino IDE: https://www.arduino.cc/en/software
Connect the transmitter Arduino to your computer and open `firmware/transmitter/transmitter.ino`.
Select the correct board and port in Tools → Board and Tools → Port.
Review `TX_PIN` and upload the sketch.
Connect the receiver Arduino and open `firmware/receiver/receiver.ino`.
Install the `LiquidCrystal_I2C` library through Sketch → Include Library → Manage Libraries (choose a compatible library).
Review `RX_PIN`, `BUZZER_PIN`, LCD address and wiring; upload the receiver sketch.
Align the transmitter LED with the optical receiver at a short distance in a controlled indoor setup.
Open the transmitter Serial Monitor at 9600 baud and send one of the sample messages, such as `BRAKE FAILURE`.
Observe the receiver LCD and buzzer. Record distance, angle, ambient light and whether the message was received.
Pin defaults
Transmitter
`TX_PIN = 3`: signal input to an LED driver/transistor stage.
Arduino GND and driver GND must be common.
Receiver
`RX_PIN = 2`: digital output from the optical receiver module.
`BUZZER_PIN = 8`: active buzzer signal.
I²C LCD: SDA/SCL pins for your Arduino board (Uno: A4/A5).
Check the actual module datasheets and adjust pins/logic levels before powering the circuit.
Environment variables
No software environment variables are required for the Arduino sketches. Configuration is done through constants at the top of each `.ino` file, such as pin numbers, timing values and LCD address. Do not place secrets or credentials in this repository.
Message format
This starter implementation sends up to 24 ASCII characters per frame. The receiver displays the decoded message and sounds the buzzer when a complete supported frame is received. Timing and optical-module polarity may need adjustment for your hardware.
Test and validation
Follow `tests/TEST_PLAN.md`. The CSV in `data/sample_messages.csv` is sample demonstration input, not recorded experimental evidence. Do not claim a tested range or reliability percentage unless you measure and document it.
Safety
Test only on a bench, not on public roads.
This system is supplementary and must never replace vehicle warning systems, safe driving, or emergency services.
Optical line-of-sight, sunlight, weather, alignment, distance and electrical noise can cause missed or incorrect messages.
Use suitable current-limiting and driver circuitry. Verify voltage compatibility before connecting modules.
Team
Team: Amethyst  
Members: R. Krishna Priya, Roshinee MK, Sri Varshini S  
Team ID: To be added
License
Released under the MIT License; see `LICENSE`.
