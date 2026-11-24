# TR-NEO (`тр-нео`) — AU & VST3 Plugin

**TR-NEO** is a virtual-analog 8-voice drum machine and 8×16 step sequencer with a modular hardware interface, built as a companion to [TB-NEO](https://github.com/ccerati/tb-neo-vst-au).

It combines a sample-accurate C++20 IEEE-754 `double` analog drum engine (`BD`, `SD`, `LT`, `HT`, `CP / RS`, `CY`, `OH`, `CH`), a shared TR-606 Global Accent Bus with multi-voice voltage droop, an SSL-style `PUNCH` bus compressor feeding 4×-oversampled ADAA1 `DRIVE` saturation with HIIR decimation, `9 × 2` stereo multi-out routing, 24 ROM kits, 6 groove styles & 4 fill generators, authentic TR-606 4-way `SCALE`, per-lane polymeter, `38 ms` flams, clock-locked Note Roll, and real-time MIDI Out.

> **Try it instantly in your browser (all OSes):** [tr-neo.pulsepatch.workers.dev](https://tr-neo.pulsepatch.workers.dev)

---

## Download & Platform Support

| Platform | Format | Status | Download |
| :--- | :--- | :--- | :--- |
| **macOS 13+** (Apple Silicon & Intel Universal) | **Audio Unit (AUv2)** & **VST3** | Available Now (`v1.0.0`) | [**Download TR-NEO-macOS-v1.0.0.zip**](https://github.com/ccerati/tr-neo-vst-au/raw/main/downloads/TR-NEO-macOS-v1.0.0.zip) |
| **Windows** (64-bit) | VST3 | Not yet supported natively | Use the [Web Audio Drum Machine](https://tr-neo.pulsepatch.workers.dev) (Chrome/Edge + Web MIDI) |
| **Linux** (x86_64 / ARM64) | VST3 / LV2 | Not yet supported natively | Use the [Web Audio Drum Machine](https://tr-neo.pulsepatch.workers.dev) (Chrome/Brave/Firefox + Web MIDI) |

*Native Windows and Linux builds are not available yet because the current plugin UI and virtual MIDI port rely on macOS-native Cocoa/WKWebView and CoreMIDI frameworks. The standalone Web Audio version runs the exact same IEEE-754 `double` analog drum DSP (`< -200 dB` null parity) directly in any desktop browser.*

### Highlights in `v1.0.0`
- **8 Discrete Circuit-Modeled Analog Voices**: Dual-oscillator ZDF Bridged-T `BD`, pitch-swept Bridged-T + swing-VCA noise `SD`, swept `LT` & `HT` with zero-bleed isolated noise tail, switchable 4-burst `CP` / 3-resonator `RS`, and a 6-oscillator PolyBLEP metallic network (`606 '81`, `606 '84 R226`, `808`) driving `CY`, `OH`, and priority-choked `CH`.
- **`9 × 2` Stereo Multi-Out Routing**: `Main Stereo` (through `PUNCH → DRIVE` Master Bus) plus 8 individual pre-master stereo Aux outputs (`BD Out`–`CH Out`) with per-strip `AUTO / MAIN / AUX` routing chips and click-free 96-sample crossfades.
- **Live Performance & Polymetric Sequencer**: Authentic TR-606 4-way `SCALE` (`1/8T`, `1/16T`, `1/16`, `1/32`), zero-drift `LINKED` vs `POLY` per-voice lane lengths (`1–16` with press-and-drag vertical control), `A / B / A→B / FILL` slots with `OFF / 4B / 8B / 16B` Auto-Fill and live `FILL NOW`, `38 ms` flams, sample-accurate clock-locked `1/8 | 1/16 | 1/32` Note Roll, `VAR LEN` polymetric groove mutation, and full-kit `CLEAR` (`A`, `B`, and `FILL`).
- **Pro Console Mixer & Real-Time MIDI Out**: Silent voice selection when `REC` is off, Solo-overrides-Mute console logic, native VST3 MIDI Out, and an optional CoreMIDI `"TR-NEO MIDI Out"` virtual port for AU hosts and external gear.

---

## Installation

### macOS (Audio Unit & VST3 — Apple Silicon + Intel)

The release archive [`TR-NEO-macOS-v1.0.0.zip`](https://github.com/ccerati/tr-neo-vst-au/raw/main/downloads/TR-NEO-macOS-v1.0.0.zip) includes Universal binaries (`arm64` + `x86_64`) for both plugin formats, plus an automated installer and uninstaller:
- **`TR-NEO.component`** — the macOS **Audio Unit (AU)** plugin *(macOS AU plugins always use `.component`, not `.au`)*
- **`TR-NEO.vst3`** — the **VST3** plugin

#### Option 1 — Automated Installer (Recommended)
1. **Download and unzip** `TR-NEO-macOS-v1.0.0.zip` completely (do not run from inside a ZIP preview).
2. **Right-click `Install TR-NEO.command` → Open**.
   - Because TR-NEO is a free indie release without paid Apple Developer notarization, macOS Gatekeeper will ask you to confirm once:
     - Click **Open** if prompted, **or**
     - On macOS 15 Sequoia and newer, open **System Settings → Privacy & Security**, scroll down to *"Install TR-NEO.command was blocked"*, and click **Open Anyway**.
3. The script copies both plugins to `~/Library/Audio/Plug-Ins/`, removes the download quarantine attribute (`xattr -cr`), and refreshes the macOS Audio Unit cache.
4. **Restart your DAW** (Ableton Live, Logic Pro, Bitwig, Reaper, Cubase, FL Studio, etc.) and rescan plugins.

#### Option 2 — Manual Install via Finder (Drag & Drop)
1. **Download and unzip** `TR-NEO-macOS-v1.0.0.zip`.
2. In **Finder**, press **`Cmd + Shift + G`**, type `~/Library/Audio/Plug-Ins/`, and press **Return**.
3. Drag the plugins into their respective folders (create the folders if they don't exist yet):
   - Drag **`TR-NEO.component`** (Audio Unit / AU) into **`Components/`**
   - Drag **`TR-NEO.vst3`** into **`VST3/`**
4. **Restart your DAW** and rescan plugins.
   - *First-time macOS security note:* Because the zip was downloaded from the web, macOS may block loading the unsigned plugin on first scan. If prompted, open **System Settings → Privacy & Security**, click **Allow Anyway** next to TR-NEO, and restart your DAW (or clear the download tag in Terminal with `xattr -cr ~/Library/Audio/Plug-Ins/Components/TR-NEO.component ~/Library/Audio/Plug-Ins/VST3/TR-NEO.vst3`).

#### Option 3 — Manual Install via Terminal
From inside the extracted `TR-NEO-macOS-v1.0.0` folder, run:

```bash
mkdir -p ~/Library/Audio/Plug-Ins/Components ~/Library/Audio/Plug-Ins/VST3
cp -R TR-NEO.component ~/Library/Audio/Plug-Ins/Components/
cp -R TR-NEO.vst3 ~/Library/Audio/Plug-Ins/VST3/
xattr -cr ~/Library/Audio/Plug-Ins/Components/TR-NEO.component ~/Library/Audio/Plug-Ins/VST3/TR-NEO.vst3
killall -9 AudioComponentRegistrar 2>/dev/null || true
```

#### Uninstall (macOS)
Run `Uninstall TR-NEO.command` from the extracted archive, or delete `TR-NEO.component` and `TR-NEO.vst3` from `~/Library/Audio/Plug-Ins/`.

---

### Windows
Native Windows VST3 builds are **not available yet**. In the meantime, you can run TR-NEO immediately with zero install in Chrome or Edge at **[tr-neo.pulsepatch.workers.dev](https://tr-neo.pulsepatch.workers.dev)** (including hardware MIDI pad/keyboard input via Web MIDI).

---

### Linux
Native Linux VST3/LV2 builds are **not available yet**. You can run TR-NEO directly in Chromium, Chrome, Brave, or Firefox at **[tr-neo.pulsepatch.workers.dev](https://tr-neo.pulsepatch.workers.dev)**.

---

## Plugin Manual — Playing with TR-NEO

### 1. Transport, DAW Sync, Pattern Slots & Auto-Fill (`МОДУЛЬ-01 // TRANSPORT`)

- **Armed on Load (`PLAY` / `ARMED` / `STOP`)**:
  - TR-NEO loads **ARMED** in **SYNC** mode so its internal 8-lane sequencer locks cleanly to Beat 1 as soon as you press Play in your DAW.
  - Clicking **PLAY** while your DAW is stopped auditions the internal sequencer immediately, then hands control back to DAW transport when the DAW starts and stops cleanly when the DAW stops.
  - Clicking **STOP** disarms the internal sequencer so you can trigger TR-NEO purely from DAW MIDI clips or external drum pads.
- **SYNC / FREE Toggle**:
  - **SYNC (lit)**: Locks tempo and step phase to your DAW's transport.
  - **FREE**: Runs on TR-NEO's internal BPM control (`60–220 BPM`) independently of DAW transport.
- **Playback Order & Swing**:
  - Switch playback direction live between `FWD` (Forward), `REV` (Reverse), `P-P` (Ping-Pong), `BRN` (Brownian walk), and `RND` (Random).
  - Dial in `0–75%` **SWING**; odd bar lengths automatically keep their unpaired final step straight so Beat 1 never drifts.
- **Pattern Variations & Auto-Fill**:
  - Switch live between **`A`**, **`B`**, and **`A→B`** alternating chain, or edit **`FILL`** directly.
  - Set Auto-Fill cadence to **`OFF`**, **`4B`**, **`8B`**, or **`16B`**, or press **`FILL NOW`** while playing to drop into Slot `FILL` immediately (exits cleanly back to your groove at the end of the bar).
- **MIDI KB Mode (`PLAY PADS` vs `PAT/FILL`)**:
  - **PLAY PADS**: Incoming DAW clips and drum pads trigger the 8 analog drum voices using the GM drum map (`Velocity ≥ 100` fires the Global Accent Bus).
  - **PAT/FILL**: Incoming MIDI notes `C3–B3` (`60–71`) control live arrangement (`60` = Slot A, `61` = Slot B, `62` = A→B Chain, `63` = `FILL NOW`, `64–71` = toggle Mute for voices `01–08`).

---

### 2. Multi-Output Routing (`9 × 2` Stereo) & Sequencer MIDI Out

TR-NEO exposes **9 stereo output buses** (1 Main + 8 individual pre-master voice Aux buses):

- **Bus 0 — `Main Stereo`**: Full mix routed through the `PUNCH → DRIVE` Master Bus.
- **Buses 1–8 — `BD Out`, `SD Out`, `LT Out`, `HT Out`, `CP Out`, `CY Out`, `OH Out`, `CH Out`**: Individual pre-master stereo outputs for DAW track processing.
- **Per-Channel Routing Chip (`AUTO` / `MAIN` / `AUX`)**:
  - **`AUTO`** (default): Automatically splits a voice to its Aux bus when connected in your DAW (with automatic bus-latch protection for hosts like Ableton Live and Reaper that pre-enable all 8 Aux buses at once so Main stays active until you explicitly split).
  - **`MAIN`**: Keeps the voice exclusively on `Main Stereo`.
  - **`AUX`**: Routes the voice exclusively to its individual Aux bus with a 96-sample click-free crossfade.
- **Routing Sequencer & Note-Roll MIDI Out**:
  - **VST3 (Ableton Live, Bitwig, Reaper, etc.)**: Sequencer and Note Roll MIDI Out are emitted natively out of the VST3 plugin. Create a second MIDI track, set its **MIDI From** input to the TR-NEO track (select `TR-NEO` in the post-FX dropdown in Ableton Live), and monitor it.
  - **Audio Unit / External Gear (CoreMIDI Virtual Port)**: Click **`MIDI PORT: OFF`** in the bottom-right footer to enable **`MIDI PORT: ON`**. A virtual macOS MIDI device named **`TR-NEO MIDI Out`** appears immediately—enable its Track input in your DAW's MIDI Preferences and select it on any track or external synth.

---

### 3. Shaping the 8 Analog Voices, Mixer & Master Bus

| Ch | Voice | Circuit Topology | Voice Controls |
| :---: | :--- | :--- | :--- |
| **01** | **BD** | Dual-oscillator ZDF Bridged-T (`60 Hz` + `265 Hz` transient click) & asymmetric op-amp rail clip | `LEVEL`, `TUNE`, `DECAY`, `SWEEP`, `PAN` |
| **02** | **SD** | Pitch-swept ZDF Bridged-T shell + swing-VCA analog noise + post-VCA HPF/LPF | `LEVEL`, `TUNE`, `DECAY`, `SNAPPY`, `PAN` |
| **03** | **LT** | Swept ZDF Bridged-T resonator + isolated shared noise tail | `LEVEL`, `TUNE`, `DECAY`, `DROP`, `PAN` |
| **04** | **HT** | Swept ZDF Bridged-T resonator + isolated shared noise tail | `LEVEL`, `TUNE`, `DECAY`, `DROP`, `PAN` |
| **05** | **CP / RS** | Switchable 4-burst BA662 **Clap** or 3-resonator **Rimshot** | `LEVEL`, `TUNE`, `DECAY`, `SPREAD`, `PAN` |
| **06** | **CY** | 6-oscillator inharmonic PolyBLEP metallic bank → dual BPF → swing-VCA → 4×/2× HIIR | `LEVEL`, `TUNE`, `DECAY`, `METAL`, `PAN` |
| **07** | **OH** | Shared 6-osc metallic network (`7100 Hz` BPF) with `1.8 ms` zero-click `CH → OH` choke | `LEVEL`, `TUNE`, `DECAY`, `METAL`, `PAN` |
| **08** | **CH** | Shared 6-osc metallic network with tight swing-VCA decay and choke priority over `OH` | `LEVEL`, `TUNE`, `DECAY`, `METAL`, `PAN` |

- **Voice Selection & Auditioning**: Clicking a mixer strip or lane selects that voice silently (no accidental sound during live sets) unless **`REC`** is armed; double-click or click the trigger pad to audition.
- **Pro Console Mute (`M`) & Solo (`S`)**: **Solo always overrides Mute**—soloing a muted channel immediately unmutes it into the solo bus rather than silencing the mix.
- **Metallic Revision & CP/RS Toggle**: Switch the shared `CY / OH / CH` metallic oscillator bank between **`606 '81`**, **`606 '84`** (`R226`), and **`808`** calibrations, and toggle Channel 05 between **Clap** (`CP`) and **Rimshot** (`RS`).
- **Master Bus (`VOLUME`, `ACCENT`, `PUNCH`, `DRIVE`)**: Global TR-606 Accent Bus (multi-voice capacitor droop), SSL-style auto-release VCA bus compressor (`PUNCH`), and 4×-oversampled ADAA1 biased-tanh saturator (`DRIVE`) with exact inverse tilt shelving and ceiling protection (`< -0.9 dBFS`).
- **Knob Precision**: Hold **`Shift` + Drag** on any knob for fine adjustment; **double-click** to reset to default.

---

### 4. Drum Kit ROM Bank (`МОДУЛЬ-02 // ROM ПЗУ-01..03`)

Switch between three ROM cartridges—**`ПЗУ-01 606`** (`606 DRUMATIX`), **`ПЗУ-02 808-909`** (`CLUB 808 / 909`), and **`ПЗУ-03 CRUNCH`** (`ANALOG CRUNCH`)—each holding **8 curated drum kits** (24 total). Kits load cleanly on pointer/touch release without overwriting your active patterns.

---

### 5. Groove Mutator, 8-Lane Step Matrix & Live Note Roll (`МОДУЛЬ-03` & `СЕКВЕНСОР 8×16`)

- **Groove & Fill Mutator**:
  - Pick a Groove Style (**`ACID TECHNO`**, **`DETROIT 909`**, **`ELECTRO 808`**, **`CHICAGO JACK`**, **`BERLIN DUB`**, **`606 BREAKS`**) and Fill Style (**`SNARE BUILD`**, **`TOM CASCADE`**, **`606 ROLL`**, **`HAT STUTTER`**), then click **`MUTATE`** to generate a fresh groove (Slot `FILL` automatically inherits the first half of your groove unless you hand-edit it).
  - Check **`VAR LEN · AUTO POLYMETER`** to automatically assign musical per-voice polymetric loop lengths on every mutation.
  - **`CLEAR`**: Immediately clears all three pattern slots (`A`, `B`, and `FILL`) for a clean slate.
- **Tactile Step Brush (`HIT` | `ACC` | `FLM`)**:
  - Paint hits by tapping or dragging across pads with **`HIT`**, **`ACC`** (Global Accent), or **`FLM`** (`38 ms` double-strike flam, auto-scaled for fast `1/32` rates).
  - Shortcuts while in `HIT` brush: **`Shift` / Right-Click** paints `ACC`, **`Alt` + Click** paints `FLM`.
- **4-Way TR-606 `SCALE` & `LINKED` / `POLY` Step Lengths**:
  - Switch timing subdivisions live between **`1/8T`** (3 steps/beat), **`1/16T`** (6 steps/beat), **`1/16`** (4 steps/beat), and **`1/32`** (8 steps/beat).
  - Toggle **`LINKED`** (shared 1–16 step length across all 8 lanes) or **`POLY`** (independent 1–16 loop length per voice via `−`/`+`, step numbers `01–16`, or the right-hand `LEN` boxes).
- **Clock-Locked Live Note Roll (`1/8` | `1/16` | `1/32`)**:
  - Hold **`1/8`**, **`1/16`**, or **`1/32`** to fire sample-accurate, clock-locked note repeats on the currently selected voice—switch voices mid-roll for instant fills and stutters.

---

### 6. DAW Automation, GM Drum MIDI Map & MIDI CC Reference

All **56 parameters** are exposed for host automation (`Master Volume`, `Global Accent`, `Master Drive`, `Master Punch`, `CP Clap/Rim`, `Seq Play`, `Seq BPM`, `Seq Swing`, `Seq Order`, `Host Sync`, `Seq Length`, `Seq Scale`, `Variation`, `Auto-Fill`, `Fill Now`, `MIDI KB Mode`, plus `Level`, `Tune`, `Decay`, `Sweep/Snappy/Drop/Spread/Metal`, and `Pan` for each of the 8 voices).

| MIDI Note | Voice | Notes |
| :--- | :--- | :--- |
| `C1` (`36`) / `B0` (`35`) | **01 BD** | Bass Drum |
| `D1` (`38`) / `E1` (`40`) | **02 SD** | Snare Drum |
| `F1` (`41`) / `G1` (`43`) | **03 LT** | Low Tom |
| `A1` (`45`) / `B1` (`47`) / `C2` (`48`) | **04 HT** | High Tom |
| `D#1` (`39`) | **05 CP** | Clap (switches Ch 05 to Clap mode) |
| `C#1` (`37`) | **05 RS** | Rimshot (switches Ch 05 to Rimshot mode) |
| `C#2` (`49`) / `D#2` (`51`) / `G2` (`55`) / `A2` (`57`) | **06 CY** | Cymbal |
| `A#1` (`46`) | **07 OH** | Open Hi-Hat |
| `F#1` (`42`) / `G#1` (`44`) | **08 CH** | Closed Hi-Hat (chokes `OH`) |

*Incoming notes with Velocity ≥ 100 fire the Global Accent Bus.*

| MIDI CC | Function |
| :--- | :--- |
| `CC 7` | Master Volume |
| `CC 71` | Global Accent |
| `CC 74` | Master Drive |
| `CC 120` | All Sound Off |
| `CC 123` | All Notes Off |
