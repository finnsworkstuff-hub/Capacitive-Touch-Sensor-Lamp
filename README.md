# Capacitive-Touch-Sensor-Lamp

This project demonstrates how a capacitive touch sensor can be implemented using an Arduino microcontroller to control an LED lamp. It detects changes in capacitance when a user touches a conductive surface (aluminium foil), switching the LED on or off.

---

- Arduino Uno  
- 1MΩ Resistor  
- Aluminium foil (touch sensor)  
- LED & 220Ω Resistor  
- Jumper wires, breadboard  

---

Capacitance is the ability of a system to store an electric charge. When a conductive object (such as a human finger) comes into contact or near proximity with a conductor, it alters the capacitance of that system.

The Arduino can detect this change using two pins: one acts as a sender and the other as a receiver. By timing how long it takes for the pins to reach the same electrical state, the CapacitiveSensor library calculates a relative capacitance value.
The charging follows:

**V(t) = Vcc(1 - e^(-t/(RC)))**

Where:
- Vcc is supply voltage
- R is the resistor value
- C is the capacitance
- e^(-t / (R C)) is an exponential decay term and it shows how something (like voltage or charge) changes rapidly at first, then slows down over time.

The constant τ tells you how fast the capacitor charges. So if C increases (because you touched the foil), τ increases.

The Arduino’s receive pin (Pin 2) reads “LOW” until the capacitor voltage reaches the threshold, when that happens, it becomes “HIGH.” The Arduino times how long this takes. We can find this by rearranging the RC equation to get:

**tₜₕ = -RC ln(1 - Vₜₕ / Vcc)**

The term in parentheses ln(1 - Vₜₕ / Vcc) is just a number and doesn’t change as long as Vₜₕ and Vcc stay the same. We could say

**tₜₕ = constant × R × C**, or
**tₜₕ ∝ RC**

The CapacitiveSensor library measures the time (or number of loops) required for the receive pin’s voltage to cross the Arduino’s digital HIGH threshold. This analog timing result is digitized by the microcontroller and compared to a predefined threshold:

**Serial.println(sensorValue);
  if(sensorValue > threshold) {
    digitalWrite(ledPin, HIGH);
  }
  else {
    digitalWrite(ledPin, LOW);
  }**

So a bigger capacitance (more touch) = longer time to reach threshold = higher sensor value = LED turns on.

When a finger approaches the sensor (in this case, aluminium foil), the human body’s capacitance absorbs part of the charge and increases the measured delay, which the Arduino detects as the threshold voltage being reached. This principle is used in touch-activated devices ranging from lamps to mobile touchscreens.

---

- Untouched: readings ≈ 100–300  
- Touched: readings ≈ 1200+  
- The LED switched reliably when threshold was adjusted to ~100  
- Even nearby phones or hands triggered small capacitance changes!

---

- Circuit design and testing  
- Arduino programming (analog input, threshold logic)  
- Understanding RC time constants  
- Debugging and calibration  
- Signal noise and stability handling  

---

## 🗂️
- `capacitive_sensor.ino` — Arduino source code   
