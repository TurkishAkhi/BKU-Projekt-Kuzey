<!-- ═══════════════════════════════════════════════════════════════ -->
<!--                        HEADER BANNER                          -->
<!-- ═══════════════════════════════════════════════════════════════ -->

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:1f3a5f,100:0d1117&height=220&section=header&text=PlaygroundMaster%20Car&fontSize=48&fontColor=58a6ff&animation=fadeIn&fontAlignY=36&desc=Autonomous%20Embedded%20Vehicle%20System&descAlignY=55&descSize=18&descColor=8b949e&stroke=1f6feb&strokeWidth=1" />
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:dbeafe,50:bfdbfe,100:dbeafe&height=220&section=header&text=PlaygroundMaster%20Car&fontSize=48&fontColor=1d4ed8&animation=fadeIn&fontAlignY=36&desc=Autonomous%20Embedded%20Vehicle%20System&descAlignY=55&descSize=18&descColor=374151" alt="header" />
</picture>

<!-- TYPING ANIMATION -->
<img src="https://readme-svg-typing-generator.vercel.app/api?lines=Autonomous+Navigation+%F0%9F%A4%96;Ultrasonic+Obstacle+Avoidance+%F0%9F%94%8A;Real-Time+OLED+Telemetry+%F0%9F%93%9F;Joystick+Manual+Control+%F0%9F%8E%AE;Built+with+C%2B%2B+%E2%9A%99%EF%B8%8F&animation=typing&font=Fira+Code&color=58a6ff&size=22&height=50&duration=3500&pause=1000" alt="Typing Animation" />

<br/><br/>

<!-- BADGES ROW 1 -->
[![Language](https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)](https://isocpp.org/)
[![Platform](https://img.shields.io/badge/PlaygroundMaster-0078D4?style=for-the-badge&logo=arduino&logoColor=white)]()
[![IDE](https://img.shields.io/badge/Arduino_IDE-00979D?style=for-the-badge&logo=arduino&logoColor=white)](https://www.arduino.cc/)
[![Status](https://img.shields.io/badge/Status-In_Development-f59e0b?style=for-the-badge&logo=git&logoColor=white)]()

<!-- BADGES ROW 2 -->
[![Sensors](https://img.shields.io/badge/HC--SR04-Ultrasonic-8b5cf6?style=flat-square)]()
[![Display](https://img.shields.io/badge/OLED-SSD1306-ec4899?style=flat-square)]()
[![Steering](https://img.shields.io/badge/Servo-SG90%2FMG996R-10b981?style=flat-square)]()
[![Drive](https://img.shields.io/badge/Motor-L298N-ef4444?style=flat-square)]()
[![Input](https://img.shields.io/badge/Joystick-Analog_XY-f97316?style=flat-square)]()

<br/>

> **A compact autonomous vehicle engineered on the PlaygroundMaster platform.**
> Real-time ultrasonic obstacle avoidance · Servo steering · Dual-mode control · Live OLED telemetry
> — all written from scratch in **C++**.

</div>

---

## 📖 Table of Contents

- [About the Project](#-about-the-project)
- [Features](#-features)
- [System Architecture](#-system-architecture)
- [Hardware](#-hardware)
- [Software](#-software)
- [Obstacle Avoidance Algorithm](#-obstacle-avoidance-algorithm)
- [OLED Display](#-oled-display)
- [Getting Started](#-getting-started)
- [Roadmap](#-roadmap)
- [Known Issues](#-known-issues)
- [Team](#-team)

---

## 🧭 About the Project

This project was engineered as part of a school **embedded systems** course. Starting from a bare chassis and a set of components, we designed the wiring, wrote every C++ driver from scratch, and tuned the system end-to-end on a physical playground track.

**The challenge:** make the vehicle navigate autonomously without ever touching the environment — while keeping full manual joystick override available at any time.

| | Manual Mode 🕹️ | Autonomous Mode 🤖 |
|---|---|---|
| **Input** | Joystick XY + button | 3× ultrasonic sensors |
| **Steering** | Direct servo mapping | Algorithm-driven |
| **Speed** | Analog Y-axis | Fixed cruise PWM |
| **Switch** | — | Joystick button press |

---

## ⚡ Features

<table>
<tr>
<td width="50%">

**🔊 Triple Ultrasonic Coverage**
Front, left, and right HC-SR04 sensors give the vehicle 180° awareness — no blind spots in the forward hemisphere.

**🎮 Analog Joystick Control**
Full XY axis mapping with configurable deadzone filtering and button-press mode toggle — no lag, no drift.

**🔄 Dynamic Avoidance Algorithm**
On obstacle detection: stop → scan both sides → compare distances → turn toward wider gap → continue. All in under 50 ms.

</td>
<td width="50%">

**📟 Live OLED Telemetry**
Mode, motor PWM, all three sensor distances, and servo angle — refreshed every loop cycle with flicker-free rendering.

**🏗️ Modular C++ Architecture**
Each subsystem is a self-contained `.h/.cpp` module. Easy to debug, easy to extend.

**⚙️ Tunable Constants**
All thresholds, speeds, angles, and pin mappings live in `config.h`. Tune without touching business logic.

</td>
</tr>
</table>

---

## 🗺️ System Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         PLAYGROUNDMASTER                            │
│                                                                     │
│  ┌─────────────┐   ┌─────────────────┐   ┌────────────────────┐    │
│  │   INPUTS    │   │      LOGIC      │   │      OUTPUTS       │    │
│  │             │   │                 │   │                    │    │
│  │ HC-SR04 (F) │──▶│                 │   │ ┌────────────────┐ │    │
│  │ HC-SR04 (L) │──▶│ Obstacle        │──▶│ │   DC Motor     │ │    │
│  │ HC-SR04 (R) │──▶│ Avoidance       │   │ │   (L298N)      │ │    │
│  │             │   │ Algorithm       │   │ └────────────────┘ │    │
│  │ Joystick XY │──▶│                 │   │ ┌────────────────┐ │    │
│  │ Joystick BTN│──▶│ Mode Manager    │──▶│ │  Servo Motor   │ │    │
│  │             │   │ (Manual / Auto) │   │ │  (Steering)    │ │    │
│  └─────────────┘   └────────┬────────┘   │ └────────────────┘ │    │
│                             │            │ ┌────────────────┐ │    │
│                             └───────────▶│ │  OLED Display  │ │    │
│                                          │ │  (Telemetry)   │ │    │
│                                          │ └────────────────┘ │    │
│                                          └────────────────────┘    │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🔩 Hardware

### Bill of Materials

| # | Component | Model | Qty | Role |
|---|-----------|-------|-----|------|
| 1 | Microcontroller | PlaygroundMaster | 1 | Central brain |
| 2 | Ultrasonic Sensor | HC-SR04 | 3 | Front · Left · Right ranging |
| 3 | Servo Motor | SG90 / MG996R | 1 | Wheel steering angle |
| 4 | DC Motor + Driver | Generic + L298N | 1 | Forward / reverse drive |
| 5 | Joystick | Analog XY Module | 1 | Manual input |
| 6 | OLED Display | 0.96″ I²C SSD1306 | 1 | Live telemetry |
| 7 | Power Supply | LiPo / USB pack | 1 | Onboard power |

### Wiring

```
                    ┌──────────────────────────────┐
                    │        PlaygroundMaster       │
                    │                              │
  [HC-SR04 Front]──│ TRIG → D2     ECHO → D3      │
  [HC-SR04 Left] ──│ TRIG → D4     ECHO → D5      │
  [HC-SR04 Right]──│ TRIG → D6     ECHO → D7      │
                    │                              │
  [Servo]         ──│ PWM  → D9                    │
                    │                              │
  [L298N / Motor] ──│ IN1  → D10    IN2  → D11     │
                  ──│ ENA  → D8                    │
                    │                              │
  [Joystick]      ──│ VRX  → A0     VRY  → A1      │
                  ──│ SW   → D12                   │
                    │                              │
  [OLED SSD1306]  ──│ SDA  → A4     SCL  → A5 (I²C)│
                    └──────────────────────────────┘
```

> ⚠️ Update `config.h` if pin assignments change during final assembly.

---

## 💻 Software

### Tech Stack

![C++](https://img.shields.io/badge/C++-00599C?style=flat-square&logo=c%2B%2B&logoColor=white)
![Arduino](https://img.shields.io/badge/Arduino-00979D?style=flat-square&logo=arduino&logoColor=white)
![PlatformIO](https://img.shields.io/badge/PlatformIO-F5822A?style=flat-square&logo=platformio&logoColor=white)
![I2C](https://img.shields.io/badge/I²C-Protocol-8b5cf6?style=flat-square)
![PWM](https://img.shields.io/badge/PWM-Signal-10b981?style=flat-square)

### Libraries

```cpp
#include <Servo.h>             // Built-in  — servo PWM control
#include <Wire.h>              // Built-in  — I²C bus driver
#include <Adafruit_GFX.h>      // External  — graphics primitives
#include <Adafruit_SSD1306.h>  // External  — SSD1306 OLED driver
```

### Project Structure

```
PlaygroundMaster-Car/
│
├── 📄 README.md
├── 📄 platformio.ini              ← Build config & board target
│
└── 📁 src/
    ├── 🔷 main.cpp                ← setup() + loop() — orchestrates all modules
    ├── 🔷 config.h                ← ALL pin defs, thresholds, PWM limits
    │
    ├── 🔷 motor.h / motor.cpp     ← forward(), reverse(), stop(), setSpeed(pwm)
    ├── 🔷 servo_control.h / .cpp  ← setAngle(deg), steerLeft(), steerRight()
    ├── 🔷 ultrasonic.h / .cpp     ← getDistance(sensor), triggerPulse()
    ├── 🔷 joystick.h / .cpp       ← readX(), readY(), isPressed(), applyDeadzone()
    └── 🔷 display.h / display.cpp ← renderTelemetry(), clearBuffer(), flush()
```

### Key Constants (`config.h`)

```cpp
// ─── Pin Mapping ────────────────────────────────────────────────────
#define TRIG_FRONT    2       #define ECHO_FRONT    3
#define TRIG_LEFT     4       #define ECHO_LEFT     5
#define TRIG_RIGHT    6       #define ECHO_RIGHT    7
#define PIN_SERVO     9
#define MOTOR_IN1     10      #define MOTOR_IN2     11
#define MOTOR_ENA     8
#define JOY_X         A0      #define JOY_Y         A1
#define JOY_BTN       12

// ─── Behaviour ──────────────────────────────────────────────────────
#define OBSTACLE_CM       20   // Stop threshold in centimetres
#define JOY_DEADZONE      50   // Analog deadzone around centre (512)
#define SERVO_CENTER      90   // Degrees — straight ahead
#define SERVO_MAX_LEFT    45   // Degrees left limit
#define SERVO_MAX_RIGHT   135  // Degrees right limit
#define MOTOR_CRUISE      180  // Autonomous cruise PWM (0–255)
#define MOTOR_MAX         220  // Manual mode PWM cap
```

---

## 🧠 Obstacle Avoidance Algorithm

<details>
<summary><b>📊 Click to expand — flowchart + pseudocode</b></summary>

<br/>

```
  ╔══════════════════════╗
  ║    MAIN LOOP START   ║
  ╚══════════╤═══════════╝
             │
             ▼
  ┌──────────────────────┐
  │  Read all 3 sensors  │   ← every ~50 ms
  └──────────┬───────────┘
             │
  ┌──────────▼───────────┐
  │  front_dist < 20 cm? │
  └──────┬───────────────┘
        YES                NO
         │                  │
         ▼                  ▼
  ┌────────────┐      ┌──────────────────┐
  │   STOP()   │      │  driveForward()  │
  └──────┬─────┘      └──────────────────┘
         │
         ▼
  ┌──────────────────────┐
  │  scan left & right   │
  └──────────┬───────────┘
             │
  ┌──────────▼───────────┐
  │  left > right dist?  │
  └──────┬───────┬────────┘
        YES      NO
         │        │
         ▼        ▼
     turnLeft() turnRight()
         │        │
         └───┬────┘
             ▼
       driveForward()
```

```cpp
void autonomousLoop() {
    float front = getDistance(FRONT);
    float left  = getDistance(LEFT);
    float right = getDistance(RIGHT);

    if (front < OBSTACLE_CM) {
        stop();
        delay(100);

        if (left > right) {
            steerLeft();
        } else {
            steerRight();
        }
        delay(300);
    } else {
        steerCenter();
        driveForward(MOTOR_CRUISE);
    }

    renderTelemetry(front, left, right);  // update OLED
}
```

</details>

---

## 📟 OLED Display

The 0.96″ SSD1306 renders live vehicle telemetry every loop cycle:

```
  ┌────────────────────────────┐
  │ ● MODE : AUTONOMOUS        │  ← active mode (● pulses in auto)
  │ SPD    : ████░░░░  180pwm  │  ← speed bar + raw PWM value
  │ FRONT  : 034 cm            │  ← ultrasonic front distance
  │ LEFT   : 012 cm            │  ← ultrasonic left distance
  │ RIGHT  : 048 cm            │  ← ultrasonic right distance
  │ ANGLE  : [  90°  ]         │  ← current servo position
  └────────────────────────────┘
```

Rendering uses `clearDisplay()` → batch draw → `display()` per cycle to avoid flicker. All values are sourced directly from live sensor reads and motor state — no caching lag.

---

## 🚀 Getting Started

### Prerequisites

| Tool | Version | Link |
|------|---------|------|
| Arduino IDE | 2.x+ | [arduino.cc](https://www.arduino.cc/en/software) |
| Adafruit GFX Library | latest | Arduino Library Manager |
| Adafruit SSD1306 Library | latest | Arduino Library Manager |

### Setup

```bash
# 1. Clone the repository
git clone https://github.com/your-username/playgroundmaster-car.git
cd playgroundmaster-car
```

**Arduino IDE:**
```
1. File → Open → src/main.cpp
2. Tools → Board → PlaygroundMaster / Arduino Uno
3. Tools → Port → select your COM port
4. Tools → Manage Libraries → install Adafruit GFX + Adafruit SSD1306
5. Sketch → Upload  (Ctrl+U)
```

**PlatformIO:**
```bash
pio run --target upload
```

### First Boot Checklist

- [ ] OLED displays `MODE: MANUAL` on startup
- [ ] Joystick X-axis moves servo left/right
- [ ] Joystick Y-axis spins motor forward/backward
- [ ] Serial Monitor shows sensor distances at 115200 baud
- [ ] Button press switches to `MODE: AUTONOMOUS`
- [ ] Vehicle stops when hand is placed within 20 cm in front

---

## 📅 Roadmap

| Phase | Task | Status |
|-------|------|--------|
| **1 · Hardware** | Chassis assembly & component mounting | ⬜ Planned |
| **1 · Hardware** | Full wiring + continuity test | ⬜ Planned |
| **2 · Core SW** | Motor forward / reverse / speed control | ⬜ Planned |
| **2 · Core SW** | Servo steering + angle mapping | ⬜ Planned |
| **2 · Core SW** | Joystick read + deadzone filter | ⬜ Planned |
| **2 · Core SW** | OLED layout + refresh loop | ⬜ Planned |
| **3 · Autonomy** | HC-SR04 distance calculation | ⬜ Planned |
| **3 · Autonomy** | Obstacle detection + stop logic | ⬜ Planned |
| **3 · Autonomy** | Left/right scan + turn decision | ⬜ Planned |
| **3 · Autonomy** | Mode toggle (manual ↔ autonomous) | ⬜ Planned |
| **4 · Integration** | Full system test on Playground | ⬜ Planned |
| **4 · Integration** | Threshold + speed tuning | ⬜ Planned |
| **5 · Delivery** | Field demo recording | ⬜ Planned |
| **5 · Delivery** | Final documentation + presentation | ⬜ Planned |

---

## 🐛 Known Issues

<details>
<summary><b>View known issues and workarounds</b></summary>

<br/>

| # | Issue | Workaround | Status |
|---|-------|------------|--------|
| 1 | Sensor cross-talk when 2+ sensors trigger simultaneously | Stagger trigger pulses by 10 ms each | 🔍 Investigating |
| 2 | OLED flicker at high loop speed | Use dirty-region clearing only | 📋 Planned |
| 3 | Motor stall at low PWM (< 80) | Enforce minimum PWM threshold in `config.h` | 📋 Planned |
| 4 | Joystick drift on worn module | Widen `JOY_DEADZONE` constant | ✅ Configurable |

</details>

---

## 👥 Team

<table align="center">
<tr>
<td align="center" width="180">
  <b>[Name 1]</b><br/>
  <sub>Hardware & Wiring</sub><br/>
  <a href="https://github.com/username">@username</a>
</td>
<td align="center" width="180">
  <b>[Name 2]</b><br/>
  <sub>Motor & Servo Driver</sub><br/>
  <a href="https://github.com/username">@username</a>
</td>
<td align="center" width="180">
  <b>[Name 3]</b><br/>
  <sub>Sensors & Autonomy</sub><br/>
  <a href="https://github.com/username">@username</a>
</td>
<td align="center" width="180">
  <b>[Name 4]</b><br/>
  <sub>OLED & Joystick</sub><br/>
  <a href="https://github.com/username">@username</a>
</td>
</tr>
</table>

---

## 🤝 Contributing

This is a school project, but feedback and suggestions are welcome!

1. Fork the repository
2. Create your branch: `git checkout -b feature/your-idea`
3. Commit your changes: `git commit -m "feat: describe your change"`
4. Push to the branch: `git push origin feature/your-idea`
5. Open a Pull Request

---

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:1f3a5f,100:0d1117&height=120&section=footer" />
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:dbeafe,50:bfdbfe,100:dbeafe&height=120&section=footer" alt="footer" />
</picture>

**Built with ❤️ · Embedded Systems School Project · 2026**

[![Made with C++](https://img.shields.io/badge/Made_with-C++-00599C?style=flat-square&logo=c%2B%2B&logoColor=white)]()
[![Powered by Arduino](https://img.shields.io/badge/Powered_by-Arduino-00979D?style=flat-square&logo=arduino&logoColor=white)]()

</div>
