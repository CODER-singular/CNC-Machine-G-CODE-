# arduino-cnc-project

A 2-axis CNC drawing machine built from salvaged DVD drive components, controlled via Arduino Uno and GRBL firmware. Built as a finals project in our second semester of computer engineering.

---

## What It Does

The machine draws vector designs on paper using a pen mounted on recycled DVD slider rails. Designs are created in AutoCAD, exported as G-code, and streamed to the machine via OpenBuilds CONTROL. A servo motor handles Z-axis pen lift between strokes.

---

## Hardware

| Component | Source |
|---|---|
| Arduino Uno | New |
| CNC Shield v3 | New |
| Stepper motors (X & Y axes) | Salvaged from DVD drives |
| Slider rails (X & Y axes) | Salvaged from DVD drives |
| Servo motor (Z-axis / pen lift) | New |
| Power supply (12V) | New |
| Jumper wires, screws, frame | Generic / fabricated |

### Wiring

Refer to `/wiring/cnc-wiring-diagram.png` for the full wiring schematic.

Key connections:
- → X-axis stepper → CNC Shield X driver
- → Y-axis stepper → CNC Shield Y driver
- → Servo signal wire → Arduino Pin 11 (Z-axis)
- → CNC Shield powered via 12V external supply
- → Arduino powered via USB during flashing, then via CNC Shield VCC

---

## Software & Firmware

**Firmware:** GRBL (flashed onto Arduino Uno via Arduino IDE)

**G-code Sender:** OpenBuilds CONTROL

**Design Software:** AutoCAD (exported as .gcode files)

### Flashing GRBL

1. Download the correct GRBL version from [grbl/grbl](https://github.com/grbl/grbl)
2. Open Arduino IDE
3. Go to `Sketch > Include Library > Add .ZIP Library` and select the GRBL zip
4. Open `File > Examples > grbl > grblUpload`
5. Select your board (Arduino Uno) and COM port
6. Upload

> **Note:** Using the wrong GRBL version or skipping steps-per-mm calibration will cause incorrect movement scaling. Make sure to configure `$100`, `$101` (steps/mm for X and Y) based on your specific stepper motors and microstepping settings.

### GRBL Configuration Used

```
$100 = 80.000   (X steps/mm)
$101 = 80.000   (Y steps/mm)
$110 = 800.000  (X max rate, mm/min)
$111 = 800.000  (Y max rate, mm/min)
$120 = 10.000   (X acceleration)
$121 = 10.000   (Y acceleration)
```

> These values are specific to our DVD stepper motors. Yours may differ — calibrate accordingly.

---

## G-code Files

Located in `/gcode/`:

| File | Description |
|---|---|
| `heart.gcode` | Heart shape drawing |
| `star.gcode` | Star shape drawing |
| `test-pattern.gcode` | Geometric calibration pattern |

All files were designed in AutoCAD and exported as G-code. To run:

1. Open OpenBuilds CONTROL
2. Connect to your Arduino via the correct COM port
3. Load the `.gcode` file
4. Home the machine manually
5. Hit Run

---

## Repo Structure

```
arduino-cnc-project/
├── gcode/
│   ├── heart.nc
│   ├── star.nc
│   └── test-pattern.nc
├── wiring/
│   └── cnc-wiring-diagram.png
└── README.md
```

---

## Known Issues & Lessons Learned

- → Salvaged stepper motors vary in step angle — measure yours before setting steps/mm
- → GRBL must be configured correctly before sending any G-code; incorrect steps/mm will produce scaled or distorted output
- → Servo PWM range may need adjustment depending on the servo used for pen lift
- → Mechanical backlash in DVD rails is noticeable at high speeds — keep feed rate conservative (600–800 mm/min worked well for us)

---

## Built By

Second semester engineering students — LCA Finals Project.
Special thanks to **Taimoor** for pointing us toward GRBL and unblocking the firmware configuration.

---

## License

MIT — use it, build on it, improve it.
