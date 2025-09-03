# ESP8266 IR Remote LED Controller

This project allows you to control an LED connected to an **ESP8266** using an **IR remote**, providing functions for turning the LED on/off, blinking a specified number of times, smooth brightness fading (PWM), and adjusting the blinking speed.

---

## ✨ Features
- **LED On/Off Control** – toggle the LED using a dedicated remote button.
- **Blinking Mode** – make the LED blink **1–9 times** depending on the button pressed.
- **Smooth Fade Effect** – gradually brighten and dim the LED using PWM.
- **Adjustable Blink Speed** – use **+** and **−** buttons to control the blinking delay.
- **Dynamic State Restoration** – after fading, the LED returns to its previous state.
- **IR Remote Support** – uses `IRremoteESP8266` to decode signals from a standard IR remote.

---

## 🛠️ Hardware Setup
| Component        | Pin (ESP8266) | Description            |
|------------------|---------------|------------------------|
| IR Receiver      | **GPIO5**     | Receives IR signals    |
| LED / Relay      | **GPIO12**    | Controls LED or relay  |
| Power Supply     | **3.3V**      | Powers ESP8266 module  |

> **Note:** The LED is **active LOW** – `digitalWrite(LOW)` turns it **on**.

---

## 📌 Remote Control Mapping
| Button | Function               |
|--------|------------------------|
| EQ     | Toggle LED On/Off      |
| Fade   | Smooth fade-in/out     |
| 1–9    | Blink LED N times      |
| +      | Increase blink speed   |
| −      | Decrease blink speed   |

---

## 💻 Code Overview
- **IR Decoding**: Uses [`IRremoteESP8266`](https://github.com/crankyoldgit/IRremoteESP8266) to receive and interpret IR signals.
- **PWM Control**: Uses `analogWrite()` for smooth brightness transitions.
- **Blink Control**: `blinkLed(times)` makes the LED blink a specified number of times.
- **Fade Mode**: `fadeLed()` smoothly brightens and dims the LED.
- **Non-blocking improvements possible**: Currently uses `delay()`, but can be optimized with `millis()` for better responsiveness.

---

## 🚀 Getting Started
1. **Install the required library**  
   - [`IRremoteESP8266`](https://github.com/crankyoldgit/IRremoteESP8266)
2. **Connect the hardware** as shown in the table above.
3. **Upload the code** to your ESP8266 board.
4. **Point your IR remote at the receiver** and control the LED.

---

## 🧩 Possible Improvements
- Replace `delay()` with `millis()` for **non-blocking LED animations**.
- Improve **repeat code detection** by using `results.repeat` instead of checking for `0xFFFFFFFFFFFFFFFF`.
- Add support for **custom remote mappings** via configuration.

---

## 📄 License
This project is open-source and available under the **MIT License**.

---

## 👤 Author
**Dominik Pałac**  
[GitHub](https://github.com/dominik2256)
