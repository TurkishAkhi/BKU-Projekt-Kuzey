<div align="center">

<!-- BANNER -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://capsule-render.vercel.app/api?type=waving&color=0:1a1a2e,100:16213e&height=200&section=header&text=PlaygroundMaster%20Car&fontSize=42&fontColor=58a6ff&animation=fadeIn&fontAlignY=38&desc=Autonomous%20Embedded%20Vehicle%20%7C%20C%2B%2B%20%7C%20Ultrasonic%20%7C%20OLED&descAlignY=58&descSize=16&descColor=8b949e"/>
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:e8f4fd,100:bee3f8&height=200&section=header&text=PlaygroundMaster%20Car&fontSize=42&fontColor=1a56db&animation=fadeIn&fontAlignY=38&desc=Autonomous%20Embedded%20Vehicle%20%7C%20C%2B%2B%20%7C%20Ultrasonic%20%7C%20OLED&descAlignY=58&descSize=16&descColor=374151" alt="PlaygroundMaster Car"/>
</picture>

<br/>

[![Language](https://img.shields.io/badge/Language-C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)](https://isocpp.org/)
[![Platform](https://img.shields.io/badge/Platform-PlaygroundMaster-0078D4?style=for-the-badge&logo=arduino&logoColor=white)]()
[![Status](https://img.shields.io/badge/Status-In%20Development-f59e0b?style=for-the-badge&logo=git&logoColor=white)]()
[![License](https://img.shields.io/badge/License-School%20Project-6b7280?style=for-the-badge)]()

<br/>

> **A compact autonomous vehicle built on the PlaygroundMaster platform.**  
> Features real-time ultrasonic obstacle avoidance, servo steering, dual-mode joystick control,  
> and live system telemetry on an OLED display — all written in C++.

<br/>

[📖 Documentation](#-about) · [🚀 Quick Start](#-getting-started) · [🗺️ Roadmap](#-roadmap) · [👥 Team](#-team)

</div>

---

## 📖 About

This project was built as part of a school embedded systems course. The goal was to engineer a fully functional autonomous vehicle from scratch — wiring every component, writing every driver, and tuning every parameter ourselves.

The vehicle operates in two modes selectable at runtime:

| Mode | Input | Behaviour |
|------|-------|-----------|
| 🕹️ **Manual** | Joystick | Full real-time control over speed and steering |
| 🤖 **Autonomous** | Ultrasonic sensors | Self-navigating with dynamic obstacle avoidance |

All runtime telemetry — mode, speed, distances, steering angle — is rendered live on a 0.96″ OLED display mounted on the chassis.

---

## ⚡ Features

- 🔊 **Triple ultrasonic sensing** — front, left, and right coverage for 180° obstacle detection
- 🎮 **Joystick manual control** — analog XY input with button-press mode toggle
- 🔄 **Dynamic avoidance algorithm** — compares side distances and picks the optimal escape direction
- 📟 **Live OLED telemetry** — speed, mode, distances, and servo angle updated every loop cycle
- 🏗️ **Modular C++ architecture** — each subsystem is a self-contained module with its own header

---

## 🛠️ Hardware

### Component Overview

| # | Component | Model | Function |
|---|-----------|-------|----------|
| 1 | **Microcontroller** | PlaygroundMaster | Central processing & I/O |
| 2 | **Ultrasonic Sensors** | HC-SR04 × 3 | Distance measurement (front, left, right) |
| 3 | **Servo Motor** | SG90 / MG996R | Steering — controls wheel angle |
| 4 | **DC Motor + Driver** | L298N | Drive — forward / reverse propulsion |
| 5 | **Joystick Module** | Analog XY + Switch | Manual input — speed & direction |
| 6 | **OLED Display** | 0.96″ I²C SSD1306 | Live telemetry output |

### Wiring Diagram

```
                        ┌─────────────────────────┐
                        │     PlaygroundMaster     │
                        │                         │
   HC-SR04 (Front) ─────┤ TRIG D2  │  ECHO D3    │
   HC-SR04 (Left)  ─────┤ TRIG D4  │  ECHO D5    │
   HC-SR04 (Right) ─────┤ TRIG D6  │  ECHO D7    │
                        │                         │
   Servo (Steering)─────┤ PWM  D9                 │
                        │                         │
   Motor Driver    ─────┤ IN1  D10 │  IN2  D11   │
   (L298N)         ─────┤ ENA  D8                 │
                        │                         │
   Joystick        ─────┤ VRX  A0  │  VRY  A1   │
                   ─────┤ SW   D12                │
                        │                         │
   OLED Display    ─────┤ SDA  A4  │  SCL  A5   │  ← I²C Bus
                        └─────────────────────────┘
```

> ⚠️ Pin assignments may change after final hardware assembly. Always update `config.h` to match.

---

## 💻 Software

### Dependencies

Install the following libraries via the Arduino Library Manager:

```
Servo.h              (built-in)
Wire.h               (built-in)
Adafruit GFX         → github.com/adafruit/Adafruit-GFX-Library
Adafruit SSD1306     → github.com/adafruit/Adafruit_SSD1306
```

### Project Structure

```
PlaygroundMaster-Car/
├── 📄 README.md
├── 📄 platformio.ini
│
└── src/
    ├── main.cpp               ← Entry point: setup() and loop()
    ├── config.h               ← Pin definitions and global constants
    │
    ├── motor.h / motor.cpp    ← DC motor: speed, direction, braking
    ├── servo_control.h/.cpp   ← Servo: angle mapping and steering
    ├── ultrasonic.h/.cpp      ← HC-SR04: trigger, echo, distance calc
    ├── joystick.h / .cpp      ← Analog read, deadzone filtering, button
    └── display.h / display.cpp ← SSD1306: layout, refresh, data binding
```

### Key Constants (`config.h`)

```cpp
// Pins
#define PIN_TRIG_FRONT    2
#define PIN_ECHO_FRONT    3
#define PIN_SERVO         9
#define PIN_MOTOR_IN1     10
#define PIN_MOTOR_IN2     11
#define PIN_MOTOR_ENA     8
#define PIN_JOY_X         A0
#define PIN_JOY_Y         A1
#define PIN_JOY_BTN       12

// Thresholds
#define OBSTACLE_DIST_CM  20      // Stop distance in cm
#define JOYSTICK_DEADZONE 50      // Analog deadzone (0–512 center)
#define SERVO_CENTER      90      // Straight-ahead angle
#define MOTOR_MAX_SPEED   200     // PWM cap (0–255)
```

---

## 🚦 Driving Modes

### 🕹️ Manual Mode

| Input | Mapping |
|-------|---------|
| Joystick Y+ | Drive forward |
| Joystick Y− | Drive backward |
| Joystick X | Steer left / right via servo |
| Button press | Switch to Autonomous mode |

### 🤖 Autonomous Mode

The vehicle reads all three sensors every loop iteration and reacts in under 50 ms:

```
┌────────────────────────────────────────────────┐
│                  MAIN LOOP                     │
└───────────────────┬────────────────────────────┘
                    │
                    ▼
          ┌──────────────────┐
          │  Read front dist │
          └────────┬─────────┘
                   │
        ┌──────────▼──────────┐
        │  front dist < 20cm? │
        └─────┬───────────────┘
             YES              NO
              │                │
              ▼                ▼
     ┌─────────────────┐   ┌──────────────┐
     │  STOP + scan    │   │ Drive forward│
     │  left & right   │   └──────────────┘
     └────────┬────────┘
              │
    ┌─────────▼──────────┐
    │  left > right?     │
    └──┬─────────────────┘
      YES       NO
       │         │
       ▼         ▼
  Turn LEFT  Turn RIGHT
       │         │
       └────┬────┘
            ▼
      Drive forward
```

---

## 📟 OLED Display Layout

```
╔════════════════════╗
║ MODE: AUTONOMOUS   ║   ← Current mode
║ SPD:  180 PWM      ║   ← Motor PWM value (0–255)
║ F:34cm  L:12cm     ║   ← Front & Left sensor
║ R:48cm  A:90°      ║   ← Right sensor & servo angle
╚════════════════════╝
```

The display refreshes every loop cycle (~50 ms) using a double-buffer strategy to avoid flickering.

---

## 🚀 Getting Started

### Prerequisites

- [Arduino IDE 2.x](https://www.arduino.cc/en/software) **or** [PlatformIO](https://platformio.org/)
- PlaygroundMaster board support package installed
- Adafruit GFX + SSD1306 libraries installed

### Clone & Flash

```bash
# 1. Clone the repository
git clone https://github.com/your-username/playgroundmaster-car.git
cd playgroundmaster-car

# 2. Open in Arduino IDE
#    File → Open → src/main.cpp

# 3. Select your board
#    Tools → Board → PlaygroundMaster (or Arduino Uno)
#    Tools → Port → select your COM port

# 4. Install libraries
#    Tools → Manage Libraries → search "Adafruit SSD1306" + "Adafruit GFX"

# 5. Upload
#    Sketch → Upload  (Ctrl+U)
```

### First Boot

1. Power on the vehicle
2. OLED shows `MODE: MANUAL` → joystick is active
3. Press joystick button → switches to `MODE: AUTONOMOUS`
4. Place vehicle on flat surface and watch it navigate

---

## 📅 Roadmap

**Phase 1 — Hardware**
- [ ] Assemble chassis and mount all components
- [ ] Complete wiring and verify all connections
- [ ] Confirm sensor readings via Serial Monitor

**Phase 2 — Core Software**
- [ ] DC motor forward / reverse / speed control
- [ ] Servo steering with angle mapping
- [ ] Joystick analog read with deadzone filtering
- [ ] OLED display initialisation and layout

**Phase 3 — Autonomy**
- [ ] HC-SR04 distance calculation (all 3 sensors)
- [ ] Obstacle detection and stop logic
- [ ] Left / right comparison and turn decision
- [ ] Smooth mode switching (manual ↔ autonomous)

**Phase 4 — Integration & Tuning**
- [ ] Full system integration test on Playground
- [ ] Threshold and speed parameter tuning
- [ ] Edge case handling (narrow corridors, dead ends)
- [ ] Code cleanup and inline documentation

**Phase 5 — Delivery**
- [ ] Final field tests and video recording
- [ ] README and code comments complete
- [ ] Project presentation prepared

---

## 🐛 Known Issues / Limitations

| Issue | Status | Notes |
|-------|--------|-------|
| Sensor cross-talk at close range | 🔍 Investigating | Stagger trigger timing between sensors |
| OLED flicker at high refresh rate | 🔍 Investigating | Implement `clearDisplay()` optimisation |
| Motor stall at low PWM | 📋 Planned | Add minimum PWM threshold in `config.h` |

---

## 👥 Team

| Name | GitHub | Role |
|------|--------|------|
| *[Name 1]* | [@username]() | Hardware assembly & wiring |
| *[Name 2]* | [@username]() | Motor & servo driver code |
| *[Name 3]* | [@username]() | Ultrasonic sensing & avoidance algorithm |
| *[Name 4]* | [@username]() | OLED display & joystick integration |

---

## 📄 License

Distributed under the terms of a school project. Not intended for commercial use.  
See [`LICENSE`](LICENSE) for more information.

---

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://capsule-render.vercel.app/api?type=waving&color=0:1a1a2e,100:16213e&height=100&section=footer&fontColor=58a6ff"/>
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:e8f4fd,100:bee3f8&height=100&section=footer" alt="footer"/>
</picture>

Made with ❤️ · Embedded Systems School Project · 2025

</div>
