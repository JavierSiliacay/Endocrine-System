# Arduino LED and Buzzer Alert System

This project demonstrates a dual-mode LED and buzzer alert system using an Arduino. It features a **Normal Mode** that responds dynamically to a potentiometer input and an **Abnormality Mode** that triggers a full alert with continuous lights and sound.

## 🔧 Hardware Components
- Arduino Uno (or compatible board)
- 1 x Potentiometer
- 1 x Push Button
- 1 x Green LED
- 1 x Red LED
- 3 x Yellow LEDs
- 1 x Buzzer (PWM-capable)
- Resistors (for LEDs and button)
- Breadboard and jumper wires

## 🧠 Modes

### ✅ Normal Mode (Default)
- **Green LED**: Blinks, speed increases with potentiometer value.
- **Yellow LEDs**: Cycle one at a time with brightness based on potentiometer input.
- **Red LED & Buzzer**:
  - Activated when potentiometer hits maximum (value 255).
  - System resets when potentiometer is turned to 0.

### ⚠️ Abnormality Mode (Toggled via Button)
- All **yellow LEDs** turn on at full brightness.
- **Green and Red LEDs** stay on continuously.
- **Buzzer** emits a constant high-pitched tone (4000 Hz).
- Potentiometer input is ignored.

## 📋 Pin Configuration
| Component   | Arduino Pin |
|-------------|-------------|
| Green LED   | D9 (PWM)    |
| Red LED     | D4          |
| Buzzer      | D10 (PWM)   |
| Yellow LED1 | D3 (PWM)    |
| Yellow LED2 | D5 (PWM)    |
| Yellow LED3 | D6 (PWM)    |
| Button      | D2          |
| Potentiometer | A0        |

## 🧾 How It Works
- The button toggles between **Normal** and **Abnormality** modes.
- The potentiometer in **Normal Mode** controls the LED behavior and triggers alerts.
- System uses PWM to control LED brightness and buzzer tone.

## 🛠️ Setup
1. Connect all components based on the pin configuration above.
2. Upload the Arduino sketch.
3. Use the potentiometer to simulate sensor input.
4. Press the button to toggle alert mode.

## 📝 License
This project is open-source and free to use under the MIT License.

---

Made with ❤️ by Javier Siliacay | USTP 🇵🇭
