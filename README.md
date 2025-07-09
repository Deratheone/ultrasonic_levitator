# 🎵 Ultrasonic Levitator - Acoustic Levitation System

[![State Level Winner](https://img.shields.io/badge/State%20Level-1st%20Prize-gold?style=for-the-badge&logo=trophy)](https://github.com)
[![Zonal Level](https://img.shields.io/badge/Zonal%20Level-2nd%20Prize-silver?style=for-the-badge&logo=trophy)](https://github.com)
[![Arduino](https://img.shields.io/badge/Arduino-Uno-teal?style=for-the-badge&logo=arduino)](https://arduino.cc)
[![Frequency](https://img.shields.io/badge/Frequency-40kHz-blue?style=for-the-badge&logo=soundcloud)](https://github.com)

> **Award-winning high school science exhibition project demonstrating acoustic levitation using ultrasonic transducers**

## 🌟 Project Highlights

This project showcases the fascinating physics of **acoustic levitation** - the ability to suspend objects in mid-air using nothing but sound waves! By creating precise standing wave patterns with ultrasonic transducers, small particles can be trapped and levitated against gravity.

### 🏆 Competition Results
- 🥇 **1st Prize** - State Level Science Exhibition
- 🥈 **2nd Prize** - Zonal Level Science Fair

---

## 📺 Demonstration Videos

### Main Demonstration

https://github.com/Deratheone/ultrasonic_levitator/raw/main/videos/ultrasonic_levitator.mp4

*Watch the complete levitation system in action*

### Close-up View  

https://github.com/Deratheone/ultrasonic_levitator/raw/main/videos/ultrasonic_levitator_closeup.mp4

*Detailed view of particles being levitated*

### Local Video Files
- [📹 Main Demo](./videos/ultrasonic_levitator.mp4) 
- [🔍 Close-up View](./videos/ultrasonic_levitator_closeup.mp4)

---

## 🔬 Scientific Principle

### Acoustic Levitation Mechanism

**Principle:**  
Uses intense sound wave radiation pressure to counteract gravity on small objects via nonlinear acoustic effects.

## Core Process:
1. **Standing Wave Generation**  
   Opposing ultrasonic transducers create interference patterns.
   
2. **Stable Trapping**  
   Objects are trapped at **pressure nodes** (minimal acoustic pressure variation).

3. **Force Balance**  
   Time-averaged radiation pressure (nonlinear Bernoulli effect) provides upward force equal to gravity.

4. **Frequency Scaling**  
   Ultrasonic frequencies (e.g., 40 kHz) match wavelength to object size (λ ≈ 8.6 mm for mm-scale particles), not object resonance.

## Key Physics:
- Standing Wave Interference  
- Acoustic Radiation Pressure  
- Gor'kov Potential Theory  
- Wavelength-Object Size Matching  
---

## 🛠️ System Architecture

### Hardware Components

| Component | Purpose | Specifications |
|-----------|---------|----------------|
| Arduino Uno | Signal Generation | 40kHz via Timer1 interrupts |
| HC-SR04 Transducers | Ultrasonic Emission | 40kHz piezoelectric elements |
| Power Supply | System Power | 5V DC source |
| Support Framework | Alignment System | Manual positioning mechanism |

### Pin Connections
- **Transducers**: Connect to analog pins A0, A1, A2, A3, A4, A5
- **Power**: All transducers VCC to Arduino 5V
- **Ground**: All transducers GND to Arduino GND

### Circuit Schematic

![Ultrasonic Levitator Schematic](./imge/ultrasonic%20sensor.jpg)

*Circuit diagram showing the connection setup for ultrasonic transducers*

> **Credit**: Schematic reference from [Edison Science Corner](https://edisonsciencecorner.blogspot.com/2020/06/blog-post_18.html) - was really helpful while building the project

---

## 💻 Software Implementation

### Precise Timer-Based Code

The system uses Timer1 interrupts for precise 40kHz frequency generation. This provides more accurate timing than the basic `tone()` function:

```cpp
byte TP = 0b10101010; // Every other port receives the inverted signal

void setup() {
  DDRC = 0b11111111; // Set all analog ports to be outputs
  
  // Initialize Timer1
  noInterrupts(); // Disable interrupts
  
  TCCR1A = 0;
  TCCR1B = 0;
  TCNT1 = 0;
  OCR1A = 200; // Set compare register (16MHz / 200 = 80kHz square wave -> 40kHz full wave)
  
  TCCR1B |= (1 << WGM12); // CTC mode
  TCCR1B |= (1 << CS10); // Set prescaler to 1 ==> no prescaling
  TIMSK1 |= (1 << OCIE1A); // Enable compare timer interrupt
  
  interrupts(); // Enable interrupts
}

ISR(TIMER1_COMPA_vect) {
  PORTC = TP; // Send the value of TP to the outputs
  TP = ~TP; // Invert TP for the next run
}

void loop() {
  // Nothing left to do here :)
}
```

### Code Files
- [`code/prototype_code.ino`](./code/prototype_code.ino) - Complete levitation code

### Key Features
- **Precise Timing**: Uses hardware Timer1 for exact 40kHz generation
- **Multiple Outputs**: Can drive transducers on analog pins A0-A5
- **Efficient**: Interrupt-based operation with minimal CPU overhead

> **Code Reference**: Timer-based implementation adapted from [Edison Science Corner](https://edisonsciencecorner.blogspot.com/2020/06/blog-post_18.html) - was really helpful while building the project

---

## 🚀 Project Versions

### Version 1.0 - Basic Prototype ✅
**Status:** Completed and Demonstrated

**Features:**
- Dual transducer setup using salvaged HC-SR04 elements
- Arduino-based 40kHz signal generation
- Manual alignment system
- Successfully levitates thermocol/foam particles

**Demonstration:** Available in project videos

### Version 2.0 - Advanced System 🏆
**Status:** Award-Winning Model (State and zonal level)

**Enhanced Features:**
- Integrated motorized conveyor belt system
- L298N motor driver for precise movement control
- Automated particle feeding mechanism
- Enhanced stability and precision

> *Unfortunately, I lost the videos and documentation for Version 2.0 during a phone data transfer*

---

## 📊 Technical Specifications

### Performance Metrics
- **Operating Frequency:** 40 kHz ± 100 Hz
- **Maximum Levitation Height:** 5-8 mm
- **Particle Size Range:** 0.5-3 mm diameter
- **Power Consumption:** < 2W
- **Setup Time:** < 5 minutes

### Supported Materials
- ✅ Thermocol/Styrofoam particles
- ✅ Small plastic beads
- ✅ Lightweight foam pieces
- ✅ Paper fragments
- ❌ Metal objects (too dense)
- ❌ Water droplets (surface tension)

---

## 🏗️ Quick Build Instructions

### Prerequisites
- Arduino Uno
- HC-SR04 ultrasonic sensor modules (for transducers)
- Jumper wires and breadboard
- Arduino IDE installed

### Simple Setup Steps

1. **Extract Transducers**
   - Carefully remove ultrasonic transducers from HC-SR04 modules
   - Keep the original wiring intact

2. **Make Connections**
   - Connect transducers to Arduino analog pins A0-A5
   - Connect VCC to Arduino 5V
   - Connect GND to Arduino GND

3. **Upload Code**
   - Download `prototype_code.ino` from the `/code/` folder
   - Upload to Arduino using Arduino IDE

4. **Physical Setup**
   - Position two transducers face-to-face, about 2-3cm apart
   - Ensure they are perfectly aligned
   - Place small thermocol particles between them

5. **Test**
   - Power on Arduino
   - Observe levitation of small particles!

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
