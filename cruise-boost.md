# BMW E36 Cruise Stalk → Boost Control (VEMS)

This project repurposes the OEM cruise control stalk from a BMW E36 to control turbo boost levels via a VEMS ECU.

The system adds a driver-controlled boost trim layer on top of the existing VEMS boost control using a simple and robust state machine.

---

# 🔧 Features

- OEM BMW cruise control stalk as input device
- Discrete boost levels (step-based control)
- Override mode for boost control
- Instant fallback to default ECU boost map
- Optional "recall last boost level"
- Designed to integrate with VEMS Anytrim (Boost Target Trim Add)
- Safe by design (fail = default boost)

---

# 🧠 System Overview


BMW Stalk → MCU → Analog (PWM+RC / DAC) → VEMS Anytrim → Boost PID → MAC Valve → Wastegate


---

# 🎮 Controls (BMW Stalk Mapping)

| Function | Wire Color | Action |
|--------|------------|--------|
| Beschl. (Plus) | Blue-Green | Increase boost level |
| Verzög. (Minus) | Blue-Yellow | Decrease boost level |
| Abruf (Resume) | Blue-White | Recall last boost level |
| AUS (Off) | Purple-White | Disable override (return to default boost) |
| Common | Blue-Black | Ground reference |

### Electrical behavior

- PLUS / MINUS / RESUME → momentary switch to GND
- AUS → normally closed (NC), opens when pressed

---

# ⚙️ State Machine

## States

- **DEFAULT**
  - ECU controls boost normally
  - No trim applied

- **OVERRIDE**
  - Boost controlled via discrete steps
  - Trim applied via VEMS Anytrim

## Transitions


DEFAULT:
PLUS → OVERRIDE (step++)
MINUS → OVERRIDE (step--)
RESUME → OVERRIDE (load last step)

OVERRIDE:
PLUS → step++
MINUS → step--
RESUME → load last step
AUS → DEFAULT


---

# 📊 Boost Strategy

Boost is controlled using:

- VEMS base boost table
- + Anytrim offset (additive)

Example:

| Step | Trim (kPa) | Result |
|------|-----------|--------|
| 0 | 0 | Base boost |
| 1 | +20 | +0.2 bar |
| 2 | +40 | +0.4 bar |
| 3 | +60 | +0.6 bar |
| 4 | +80 | +0.8 bar |

---

# 🔌 Hardware

## MCU (recommended)

### 🥇 STM32 (recommended)
- STM32G4 / F3 / F4 series
- Hardware timers for stable PWM
- Optional DAC on some variants

### 🥈 ESP32
- Built-in DAC (on some variants)
- Easy development
- Slightly noisier environment

---

## Output to VEMS

### Option A (recommended for simplicity)
PWM + RC filter:


MCU PWM → 1k resistor → VEMS analog input
↓
10µF
↓
GND


### Option B
- External DAC (MCP4725 etc.)

---

## VEMS Integration

- Anytrim mode: **Boost Tgt Trim (add)**
- Input: analog voltage (0–5V)

---

## MAC Valve (Boost Solenoid)

### Electrical

- Pin 1 → +12V
- Pin 2 → VEMS SpecFET (EC18-6)

### Protection

- Flyback diode required:


+12V ---- MAC ----+---- VEMS output
|
diode
|
+12V


---

# ⚠️ Safety Considerations

- Default state = no boost trim
- Clamp boost levels in both MCU and VEMS
- Configure:
  - Overboost cut
  - IAT-based reduction
  - RPM-based limits (recommended)

---

# 📈 Future Improvements

- Push-to-pass (long press RESUME)
- CAN integration with VEMS
- Display (OLED / CAN dash)
- RPM + MAP feedback integration
- Gear-based boost limits

---

# 🚀 Tech Stack

### Firmware
- C / C++
- STM32 HAL / LL or Arduino framework (optional)

### Tools
- STM32CubeIDE / PlatformIO
- VemsTune

---

# 📂 Project Structure (suggested)


/src
main.cpp
state_machine.cpp
input_handler.cpp
output_driver.cpp

/hardware
schematic.png
wiring.md

/docs
diagrams
tuning_notes.md


---

# 🧪 Development Notes

- Start with PWM + RC (fastest iteration)
- Verify Anytrim response in VemsTune before driving
- Test boost in open-loop first
- Log boost vs duty before enabling closed-loop

---

# 🧠 Philosophy

This project aims to:

- Reuse OEM hardware for driver interaction
- Keep ECU in control of safety-critical systems
- Add functionality without compromising reliability

---

# ⚡ License

MIT (or your choice)

---

# 👊 Credits

- BMW for overengineering stalk switches
- VEMS community
- You, for making this way cooler than stock
