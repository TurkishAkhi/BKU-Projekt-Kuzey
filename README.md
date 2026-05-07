<div align="center">

<img src="https://img.shields.io/badge/Platform-PlaygroundMaster-blue?style=for-the-badge" />
<img src="https://img.shields.io/badge/Language-C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white" />
<img src="https://img.shields.io/badge/Status-In%20Development-yellow?style=for-the-badge" />
<img src="https://img.shields.io/badge/License-School%20Project-lightgrey?style=for-the-badge" />

<br/>
<br/>

# 🚗 PlaygroundMaster Autonomous Car

**An embedded C++ project featuring ultrasonic obstacle avoidance, servo steering, joystick control, and real-time OLED display feedback.**

</div>

---

## 📖 Table of Contents

- [About the Project](#-about-the-project)
- [Hardware Components](#-hardware-components)
- [Wiring Overview](#-wiring-overview)
- [Software Architecture](#-software-architecture)
- [Driving Modes](#-driving-modes)
- [Obstacle Avoidance Algorithm](#-obstacle-avoidance-algorithm)
- [OLED Display](#-oled-display)
- [Getting Started](#-getting-started)
- [Roadmap](#-roadmap)
- [Team](#-team)

---

## 🧭 About the Project

This project was developed as part of a school embedded systems course. The goal is to design and program a small autonomous vehicle using the **PlaygroundMaster** platform and **C++**.

The vehicle is capable of two operating modes:

- **Manual mode** — controlled in real time via a joystick
- **Autonomous mode** — self-navigating using ultrasonic distance sensors

All live data (speed, mode, distances, steering angle) is displayed on a compact **OLED screen** mounted on the vehicle.

---

## 🛠️ Hardware Components

| Component | Model | Role |
|---|---|---|
| Microcontroller | PlaygroundMaster | Main control unit |
| Ultrasonic Sensors | HC-SR04 × 3 | Front, left & right distance measurement |
| Servo Motor | SG90 / MG996R | Steering control |
| DC Motor + Driver | L298N | Drive (forward / backward) |
| Joystick Module | Analog XY + Button | Manual control input |
| OLED Display | 0.96" I2C SSD1306 | Live status display |

---

## 🔌 Wiring Overview

```
PlaygroundMaster
│
├── Ultrasonic Front   → TRIG: D2  | ECHO: D3
├── Ultrasonic Left    → TRIG: D4  | ECHO: D5
├── Ultrasonic Right   → TRIG: D6  | ECHO: D7
│
├── Servo (Steering)   → PWM: D9
│
├── Motor Driver       → IN1: D10 | IN2: D11 | ENA: D8
│
├── Joystick           → VRX: A0  | VRY: A1  | SW: D12
│
└── OLED Display       → SDA: A4  | SCL: A5  (I2C)
```

> ⚠️ Pin assignments may vary depending on final hardware layout. Update `config.h` accordingly.

---

## 💻 Software Architecture

### Libraries Used

```cpp
#include <Servo.h>            // Servo control
#include <Wire.h>             // I2C communication
#include <Adafruit_GFX.h>     // Graphics library
#include <Adafruit_SSD1306.h> // OLED driver
```

### File Structure

```
PlaygroundMaster-Car/
│
├── src/
│   ├── main.cpp               # Entry point — setup() & loop()
│   ├── config.h               # Pin definitions & global constants
│   ├── motor.h / .cpp         # DC motor control (speed, direction)
│   ├── servo_control.h / .cpp # Servo steering logic
│   ├── ultrasonic.h / .cpp    # Distance measurement via HC-SR04
│   ├── joystick.h / .cpp      # Joystick input processing
│   └── display.h / .cpp       # OLED screen rendering
│
├── README.md
└── platformio.ini / *.ino
```

---

## 🚦 Driving Modes

### Manual Mode (Joystick)

| Joystick Axis | Action |
|---|---|
| Y-Axis forward | Motor speed increase — drive forward |
| Y-Axis backward | Motor reverse — drive backward |
| X-Axis left/right | Servo angle — steer left or right |
| Button press | Toggle between Manual and Autonomous |

### Autonomous Mode

In autonomous mode, the vehicle navigates independently using its three ultrasonic sensors. It continuously measures distances and reacts to obstacles in real time.

---

## 🧠 Obstacle Avoidance Algorithm

```
┌─────────────────────────────┐
│       AUTONOMOUS LOOP       │
└──────────────┬──────────────┘
               │
               ▼
   ┌───────────────────────┐
   │  Read front distance  │
   └───────────┬───────────┘
               │
     ┌─────────▼──────────┐
     │  distance < 20 cm? │
     └──────┬──────┬──────┘
           NO     YES
            │       │
            ▼       ▼
       Drive    ┌───────────────────┐
      Forward   │ Stop & scan sides │
                └────────┬──────────┘
                         │
            ┌────────────▼────────────┐
            │ left dist > right dist? │
            └───────┬─────────┬───────┘
                   YES        NO
                    │          │
                    ▼          ▼
               Turn LEFT   Turn RIGHT
                    │          │
                    └────┬─────┘
                         ▼
                   Drive Forward
```

```cpp
// Pseudocode
if (frontDistance < THRESHOLD_CM) {
    stopMotor();
    if (leftDistance > rightDistance) {
        turnLeft();
    } else {
        turnRight();
    }
} else {
    driveForward();
}
```

---

## 📟 OLED Display

The OLED screen provides live feedback while the vehicle is running:

```
┌─────────────────────┐
│  MODE: AUTONOMOUS   │
│  SPEED: 180 PWM     │
│  F: 34cm  L: 12cm   │
│  R: 48cm  ANG: 90°  │
└─────────────────────┘
```

| Field | Description |
|---|---|
| `MODE` | Current driving mode |
| `SPEED` | Motor PWM value (0–255) |
| `F / L / R` | Front, left, right sensor distances |
| `ANG` | Current servo steering angle |

---

## 🚀 Getting Started

### Prerequisites

- [Arduino IDE](https://www.arduino.cc/en/software) or [PlatformIO](https://platformio.org/)
- Adafruit SSD1306 Library
- Adafruit GFX Library

### Setup

```bash
# Clone the repository
git clone https://github.com/your-username/playgroundmaster-car.git

# Open in Arduino IDE or PlatformIO
# Select the correct board (PlaygroundMaster / Arduino Uno)
# Install required libraries via the Library Manager
# Upload to board
```

---

## 📅 Roadmap

- [x] Project planning & component selection
- [ ] Hardware assembly & wiring
- [ ] Basic motor & servo control
- [ ] Joystick input processing
- [ ] Ultrasonic distance reading
- [ ] OLED display integration
- [ ] Autonomous obstacle avoidance logic
- [ ] Mode switching (manual ↔ autonomous)
- [ ] Full system integration & field testing
- [ ] Parameter tuning & final optimisation
- [ ] Documentation & presentation

---

## 👥 Team

| Name | Responsibility |
|---|---|
| *[Name 1]* | Hardware assembly & wiring |
| *[Name 2]* | Motor & servo programming |
| *[Name 3]* | Ultrasonic sensors & autonomy algorithm |
| *[Name 4]* | OLED display & joystick integration |

---

<div align="center">

Made with ❤️ as part of a school embedded systems project.

</div>
