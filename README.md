# Rocky_projecthailmary
An oled rocky dialogues played with df module 
# 🪨 Rocky Button Box — Project Hail Mary

A physical fan-made tribute device for **Rocky** from *Project Hail Mary* by Andy Weir.  
Press a button → Rocky speaks → his face lights up → his words scroll across a tiny screen.

---

## 📸 What It Does

- **0.96" OLED** shows Rocky's bitmap portrait at all times
- **0.91" OLED** displays the caption/dialogue synced to the audio
- **DFPlayer Mini** plays one of 6 randomly selected Rocky audio clips
- **Button** triggers a random clip on every press

---

## 🧰 Hardware

| Component | Details |
|---|---|
| Microcontroller | ESP32 (any 30/38-pin variant) |
| Display 1 | 0.96" SSD1306 OLED (128×64) |
| Display 2 | 0.91" SSD1306 OLED (128×32) |
| Audio Module | DFPlayer Mini |
| Speaker | 4Ω / 8Ω small speaker |
| Button | Momentary push button |
| Resistor | 1kΩ between ESP32 TX2 and DFPlayer RX |
| Power | 3.3V / 5V from ESP32 dev board |

---

## 🔌 Wiring

### 0.96" OLED — Rocky Image (I2C Bus 0)

| OLED Pin | ESP32 Pin |
|---|---|
| SDA | GPIO 21 |
| SCL | GPIO 22 |
| VCC | 3.3V |
| GND | GND |

### 0.91" OLED — Captions (I2C Bus 1)

| OLED Pin | ESP32 Pin |
|---|---|
| SDA | GPIO 25 |
| SCL | GPIO 26 |
| VCC | 3.3V |
| GND | GND |

> Both OLEDs share the same I2C address `0x3C` — this is fine because they run on **separate hardware I2C buses**.

### DFPlayer Mini

| DFPlayer Pin | ESP32 Pin |
|---|---|
| RX | GPIO 17 *(through 1kΩ resistor)* |
| TX | GPIO 16 |
| VCC | 5V |
| GND | GND |
| SPK_1 / SPK_2 | Speaker + / Speaker − |

### Button

| | |
|---|---|
| One leg | GPIO 15 |
| Other leg | GND |

*(Internal pull-up is enabled in code — no external resistor needed)*

---

## 📁 SD Card Setup (for DFPlayer Mini)

The DFPlayer reads files from a **FAT32 micro SD card**.  
Files **must** be placed in a folder called `01` and named exactly as shown:

```
SD Card
└── 01
    ├── 001.mp3
    ├── 002.mp3
    ├── 003.mp3
    ├── 004.mp3
    ├── 005.mp3
    └── 006.mp3
```

---

## 🗣️ Audio & Captions

| File | Rocky says | Caption behaviour |
|---|---|---|
| `001.mp3` | *"rocky watch crew die, could not fix, grace say grace will die, rocky fix"* | Scrolls in timed chunks (11s total) |
| `002.mp3` | *"question"* | Holds for ~2s |
| `003.mp3` | *"fist my bump"* | Holds for ~2s |
| `004.mp3` | *"it is time go"* | Holds for ~2s |
| `005.mp3` | *"Amaze Amaze Amaze!"* | Holds for ~3s |
| `006.mp3` | *"it is not enough"* | Holds for ~2s |

### 001.mp3 Caption Scroll Timing

| Chunk | Text | Duration on screen |
|---|---|---|
| 1 | `rocky watch` | 1 second |
| 2 | `crew die,` | 2 seconds |
| 3 | `could not fix` | 2 seconds |
| 4 | `grace say` | 1 second |
| 5 | `grace will die,` | 3 seconds |
| 6 | `rocky fix` | 2 seconds |

---

## 💻 Software

### Arduino Libraries Required

Install all three via **Arduino IDE → Library Manager**:

- `Adafruit SSD1306`
- `Adafruit GFX Library`
- `DFRobotDFPlayerMini`

### Board Setup

1. Install the **ESP32 board package** in Arduino IDE  
   *(File → Preferences → add `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`)*
2. Select **ESP32 Dev Module** (or your specific board) under Tools → Board
3. Set Upload Speed to `115200`

### Uploading

1. Clone or download this repo
2. Open `rocky_hailmary.ino` in Arduino IDE
3. Connect your ESP32 via USB
4. Hit **Upload**

---

## 🗂️ File Structure

```
rocky-button-box/
├── rocky_hailmary.ino    ← Main Arduino sketch
└── README.md             ← This file
```

---

## ⚙️ Customisation

**Change volume** — find this line in the sketch and set 0–30:
```cpp
dfPlayer.volume(25);
```

**Change caption font size threshold** — in `showCaption()`:
```cpp
if (len <= 10) {   // strings shorter than this use big font
```

**Change button pin** — top of sketch:
```cpp
#define BUTTON_PIN  15
```

---

## 📜 Notes

- Dialogues are kept **exactly as Rocky speaks them** in the film — unconventional grammar is intentional
- The bitmap was converted from a Rocky image using [image2cpp](https://javl.github.io/image2cpp/) at 128×64, black & white threshold mode
- If your OLED address is `0x3D` instead of `0x3C`, change `OLED_ADDR` at the top of the sketch

---

## 🙏 Credits

- *Project Hail Mary* by **Andy Weir**
- Rocky — the best alien in fiction

---

*"Amaze Amaze Amaze!"*
