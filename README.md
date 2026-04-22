# ⌨️ Macro Keypad Configurator

A fully self-contained, browser-based configurator for the **MINI KeyBoard** — a 6-key macro pad with a rotary encoder (knob). No installation required. Open the HTML file in Chrome or Edge and you're ready to go.

---

## Features

- **3 independent layers** — switch between default, secondary, and tertiary profiles per key
- **4 action types per key**
  - `Shortcut` — any modifier + key combination (Ctrl, Shift, Alt, Win)
  - `Media` — Play/Pause, Stop, Next, Prev, Vol Up/Down, Mute, Brightness
  - `Macro` — text or keystroke sequences with configurable delay
  - `App` — common system shortcuts (Screenshot, Task Manager, Explorer, Lock, etc.)
- **Knob support** — configure CCW turn, CW turn, and Press independently
- **Live Detection** — press physical keys with detection mode on to flash the matching pad key in the UI
- **Key Capture** — click "Capture" on any slot and press the physical key to auto-map it
- **Per-preset color themes** — the UI color scheme automatically changes to match the loaded preset (Media, Photoshop, Gaming, etc.)
- **WebHID integration** — send config directly to the device over USB (Chrome/Edge only), with tools to send active layer, all layers at once, cycle/reopen the HID device, scan the HID descriptor, and run a diagnostic write
- **Key Map & Assignments panel** — table showing each key's detected code, assignment, firmware packet, and status side by side
- **Import / Export** — save and share profiles as JSON
- **6 built-in presets** — Media, Productivity, Photoshop, Browser, Gaming, MS Excel
- **Collapsible device layout panel** — click the header to hide/show the visual key layout
- **Persistent storage** — profiles saved to `localStorage` automatically

---

## Usage

1. Open `keypad-configurator-v3.2.html` in **Chrome** or **Edge**
2. *(Optional)* Click **Connect WebHID** to pair the physical device over USB
3. Select a **Layer** tab (1, 2, or 3)
4. Click any key or knob control in the **Device Layout** to open its editor
5. Pick an action type, configure it, and click **✓ Save Assignment**
6. When done, click **🔄 Send New Config to Active Layer** to write to the pad (or **Send All Layers** to write all three at once)

> **Note:** WebHID is only available in Chromium-based browsers (Chrome, Edge). Key detection via keyboard events works in any browser.

---

## File Structure

```
Keyboard_Config/
├── keypad-configurator-v3.2.html   # Main app (single-file, v3.2.2)
└── README.md                        # This file
```

The entire application is a **single HTML file** with no external dependencies — all CSS, JavaScript, and markup are self-contained.

---

## Device Info

| Field       | Value              |
|-------------|--------------------|
| Device name | MINI KeyBoard      |
| App version | v3.2.2             |
| OS          | Windows            |
| USB VID     | `0x1189`           |
| USB PID     | `0x8890`           |
| Keys        | 6 + 1 rotary knob  |
| HID API     | WebHID (report ID `0x03`) |

---

## Keyboard Layout

```
┌──────┐ ┌──────┐ ┌──────┐     ╭──────╮
│  K1  │ │  K2  │ │  K3  │    (  Knob )
├──────┤ ├──────┤ ├──────┤    ( Press )
│  K4  │ │  K5  │ │  K6  │     ╰──────╯
└──────┘ └──────┘ └──────┘   ◀ CCW  CW ▶
```

---

## Firmware Packet Format

Config is sent as a **64-byte HID output report** (report ID `0x03`). Keys are encoded as:

```
type=02  mod=<modifier byte>  code=<HID keycode>
```

Media keys use HID Consumer Control codes. The device reads the config from USB and stores it internally.

---

## Browser Compatibility

| Feature              | Chrome | Edge | Firefox | Safari |
|----------------------|--------|------|---------|--------|
| Key config UI        | ✅     | ✅   | ✅      | ✅     |
| Live key detection   | ✅     | ✅   | ✅      | ✅     |
| WebHID send to device| ✅     | ✅   | ❌      | ❌     |

---

## License

MIT — feel free to fork, modify, and redistribute.
