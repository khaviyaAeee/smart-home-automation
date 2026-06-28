# Smart Home Automation

An ESP32-based home automation system that controls lights, a fan, and a power outlet from a phone or browser — over your home WiFi, with a local web dashboard and optional Firebase sync for multi-device control.

![Status](https://img.shields.io/badge/status-firmware_functional-1D9E75?style=flat-square)
![Platform](https://img.shields.io/badge/platform-ESP32-3D5A80?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-555?style=flat-square)

**Developer:** Khaviya A · Panimalar Engineering College, Poonamallee, Chennai

---

## What it does

Four relay-switched channels — two lights, a fan, and a general-purpose outlet — controlled from a clean toggle-switch dashboard that works on any phone or laptop on the same network. No app to install: the ESP32 hosts the dashboard itself as a web page.

Flip a switch in the browser, the ESP32 receives the request over WiFi, switches the matching relay, and the physical appliance turns on or off. State is also pushed to Firebase, so if you open the dashboard on a second device, both stay in sync.

---

## System architecture

<p align="center"><img src="System Architecture.webp" alt="Architecture diagram: four appliances connect through a relay module to an ESP32, which hosts a dashboard and syncs to Firebase" width="100%"></p>

<table>
<tr>
<td valign="top" width="33%">

**Appliances**

- Light 1 & Light 2
- Fan
- General-purpose outlet

Each wired through one channel of a 4-relay module.

</td>
<td valign="top" width="33%">

**ESP32**

Runs a lightweight web server that serves the dashboard, exposes `/relay` and `/status` endpoints, and drives the relay GPIOs directly — no separate microcontroller needed.

</td>
<td valign="top" width="34%">

**Firebase (optional)**

Realtime Database mirrors relay state so multiple browser tabs, devices, or a future voice-assistant integration all see the same state.

</td>
</tr>
</table>

---

## Dashboard preview

The dashboard is a single page with a toggle switch per appliance — built to be readable at a glance and usable one-handed on a phone.

```
┌─────────────────────────────┐
│  Smart Home                  │
│                               │
│  💡 Light 1          ◯───●   │
│  💡 Light 2          ●───◯   │
│  🌀 Fan              ◯───●   │
│  🔌 Outlet           ◯───●   │
└─────────────────────────────┘
```

A standalone demo version (no hardware required) lives in [`dashboard/web/`](dashboard/web/) — open `index.html` directly in a browser to try the UI with simulated state.

---

## Hardware

| Spec | Detail |
|---|---|
| **Microcontroller** | ESP32 Dev Module (WiFi + BLE) |
| **Switching** | 4-channel relay module, active LOW |
| **Control pins** | GPIO 26, 27, 14, 12 (see `firmware/esp32_main/config.h`) |
| **Power** | ESP32 powered independently from the relay-switched line |
| **Cost (approx.)** | ~₹1,270 for a full prototype kit |

Full wiring instructions: [`hardware/wiring/wiring-guide.md`](hardware/wiring/wiring-guide.md)
Bill of materials: [`hardware/bom/bill-of-materials.md`](hardware/bom/bill-of-materials.md)

⚠️ If wiring to real mains-voltage appliances rather than low-voltage test loads, see the safety note in the wiring guide — this should only be done by someone comfortable working with live electrical circuits.

---

## Getting started

### 1. Flash the firmware

1. Open `firmware/esp32_main/esp32_main.ino` in the Arduino IDE (or PlatformIO)
2. Install the required libraries: `WiFi`, `WebServer`, `HTTPClient`, `ArduinoJson` (via Library Manager)
3. Edit `firmware/esp32_main/config.h` with your WiFi credentials and Firebase project details
4. Select your ESP32 board, select the correct port, and upload
5. Open the Serial Monitor at 115200 baud — the device will print its IP address once connected

### 2. Wire the relays

Follow [`hardware/wiring/wiring-guide.md`](hardware/wiring/wiring-guide.md) to connect the relay module to the ESP32 and your test loads or appliances.

### 3. Open the dashboard

Visit the IP address printed in the Serial Monitor (e.g. `http://192.168.1.50`) from any device on the same WiFi network. Toggle switches to control appliances in real time.

---

## Repository layout

```
smart-home-automation/
├── firmware/
│   └── esp32_main/        ESP32 sketch — web server, relay control, Firebase sync
├── dashboard/
│   └── web/                Standalone dashboard demo (no hardware required)
├── hardware/
│   ├── wiring/              Wiring guide and pin mapping
│   └── bom/                 Bill of materials
├── docs/
│   └── images/               Architecture diagrams
└── LICENSE
```

---

## Where things stand right now

- [x] System architecture and pin mapping
- [x] ESP32 firmware — WiFi connection, relay control, local web dashboard
- [x] Firebase REST sync (push on toggle, periodic pull)
- [x] Standalone dashboard demo (works without hardware)
- [x] Wiring guide and bill of materials
- [ ] Physical prototype build and testing
- [ ] Voice assistant integration (Google Home / Alexa webhook)
- [ ] Scheduling (timer-based on/off)

---

## Developer

**Khaviya A**
Panimalar Engineering College, Poonamallee, Chennai

## License

[MIT](LICENSE)
