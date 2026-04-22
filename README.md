# ⌨️ Macro Keypad Configurator

A fully self-contained, browser-based configurator for the **MINI KeyBoard** — a 6-key macro pad with a rotary encoder (knob). No installation required. Open the HTML file in Chrome or Edge and you're ready to go.

---

## Features

- **Named profiles** — create, rename, duplicate, and delete custom profiles; profiles persist automatically via `localStorage`
- **4 action types per key**
  - `Shortcut` — any modifier + key combination (Ctrl, Shift, Alt, Win)
  - `Media` — Play/Pause, Stop, Next, Prev, Vol Up/Down, Mute, Brightness
  - `Macro` — text or keystroke sequences with configurable delay
  - `App` — common system shortcuts (Screenshot, Task Manager, Explorer, Lock, etc.)
- **Knob support** — configure CCW turn, CW turn, and Press independently
- **Live Detection** — press physical keys with detection mode on to flash the matching pad key in the UI
- **Key Capture** — click "Capture" on any slot and press the physical key to auto-map it
- **Per-preset color themes** — the UI color scheme automatically changes to match the loaded preset (Media, Photoshop, Gaming, etc.)
- **WebHID integration** — send config directly to the device over USB (Chrome/Edge only)
- **Advanced HID tools** — send active profile, send all layers, cycle/reopen the HID device, scan the HID descriptor, diagnostic write with full log download
- **Key Map & Assignments panel** — table showing each key's detected code, assignment, firmware packet, and status side by side
- **Import / Export** — save and load profiles as JSON; reset current profile to NULL
- **6 built-in presets** — Media, Productivity, Photoshop, Browser, Gaming, MS Excel
- **Collapsible panels** — Device Layout and Key Map cards can be collapsed to save space

---

## Usage

1. Open `keypad-configurator-v3.2.html` in **Chrome** or **Edge**
2. *(Optional)* Click **Connect WebHID** to pair the physical device over USB
3. Select or create a **Profile** using the profile bar at the top of the page
4. Click any key or knob control in the **Device Layout** to open its editor on the right
5. Pick an action type, configure it, and click **✓ Save Assignment**
6. When done, click **🔄 Send New Config to Active Layer** to write to the pad

> **Note:** WebHID is only available in Chromium-based browsers (Chrome, Edge). Key detection via keyboard events works in any browser.

---

## Managing Profiles

Profiles are the main way to organize your key configurations. The **Profile Bar** runs across the top of the page and contains all profile controls.

- **Dropdown** — switch between your saved profiles and the built-in presets. The active profile name is shown next to each assignment in the layout.
- **＋ New** — creates a blank profile. You'll be prompted to give it a name.
- **💾 Save Copy** — duplicates the current profile under a new name. Use this to create an editable version of a built-in preset.
- **✏ Rename** — renames the current profile (not available for built-in presets).
- **🗑 Delete** — deletes the current profile. At least one user profile must remain.
- **⚡ Built-in Presets** — opens the preset overlay to load one of the 6 built-in profiles.

> **Built-in presets** (Media, Productivity, Photoshop, Browser, Gaming, MS Excel) are read-only. To customize one, load it and then click **💾 Save Copy** to create your own editable version.

All profile changes are saved to `localStorage` automatically — no manual save step needed.

---

## Configuring a Key or Knob

1. Click any key (K1–K6) or knob control (Press, CCW, CW) in the **Device Layout** panel
2. The **Key Editor** opens on the right side of the page
3. Choose one of the four action types using the tabs at the top of the editor:

| Type     | What it does |
|----------|-------------|
| ⌨ Shortcut | Sends a key + modifier combo (e.g. Ctrl+Z, Win+Shift+S) |
| 🎵 Media   | Sends a media command (Play/Pause, Vol Up/Down, Mute, etc.) |
| 📝 Macro   | Types a text string or key sequence with optional delay |
| 🖥 App     | Triggers a system shortcut (Screenshot, Task Manager, Lock, etc.) |

4. Configure the action, then click **✓ Save Assignment** — the key label in the layout updates immediately
5. To remove an assignment, open the editor for that key and click **✗ Clear**

**Key Capture:** Enable **Live Detection** in the bar at the top, then click **Capture** on any key slot and press the physical key — it will auto-detect and map the keycode.

---

## Import / Export

The **Import / Export** card (bottom right) lets you:

- **📂 Import JSON** — load a previously exported profile file
- **💾 Export JSON** — save the current profile to a `.json` file for backup or sharing
- **🗑 Reset Profile to NULL** — clears all assignments in the current profile (sends nulls to device if connected)

---

## Sending Config to the Device

The **Send to Dongle** card handles USB communication. Requires **Connect WebHID** first.

| Button | Action |
|--------|--------|
| 🔄 Send New Config to Active Layer | Writes the current profile to the device |
| 🔄 Send All Layers | Writes all stored layers to the device |
| 🔁 Cycle Selected HID Device | Selects the next available HID device |
| ♻️ Reopen Current HID Device | Closes and re-opens the current HID connection |
| 🔍 Scan HID Descriptor | Reads and logs the device's HID descriptor |
| 🧪 Diagnostic Write + Save Full Log | Writes Layer 1 and saves a full debug log |
| 📝 Download Full Log (.txt) | Downloads the full session log as a text file |

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
