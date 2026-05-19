# dBmeter

A real-time noise level meter dashboard that runs entirely in the browser. Measures ambient sound in dB, visualizes the live waveform, tracks session statistics, and keeps a 7-day history — all in a single HTML file with no build step and no external libraries.

## Features

- **Real-time dB measurement** from the device microphone using the Web Audio API
- **Live waveform** visualization (time-domain) that recolors based on the current noise level
- **Session statistics** — current, average, and peak dB plus a live timer
- **Session history chart** — last 300 samples drawn on canvas with grid lines at 30, 60, 90 dB
- **Weekly report** — bar chart of daily averages for the last 7 days, persisted in `localStorage`
- **CSV export** of the weekly data
- **Horizontal reference scale** showing common noise levels (breathing, whisper, conversation, traffic, motorcycle, rock concert)
- **Single-screen dashboard layout** optimized for 1920×1080 (with a responsive fallback below 1100px)

## Stack

- Pure HTML + Vanilla JS + CSS — one `index.html`, no build, no dependencies
- Web Audio API for microphone capture and RMS → dB calculation
- Canvas 2D for the waveform, history line chart, and weekly bar chart
- `localStorage` for weekly persistence (key: `dbmeter_weekly`)
- Pretendard Variable font via CDN

## Usage

Open `index.html` in any modern browser (Chrome, Firefox, Safari, Edge), or serve the directory with any static file server:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

Click **Start**, grant microphone permission, and the meter begins reading. Click **Stop** to end the session — the session average is appended to today's bucket in the weekly report. Click **Export CSV** to download the last 7 days as `dbmeter-weekly-YYYY-MM-DD.csv`.

## Layout

```
┌──────────────────────────────────────────────────────────────────┐
│  dBmeter            [ Start ] [ Export ]      ● Idle    00:00    │
├──────────────────┬─────────────────────────┬─────────────────────┤
│                  │   Live waveform         │                     │
│      72 dB       │                         │   Weekly report     │
│                  ├─────────────────────────┤                     │
│   Sound level    │   Session history       │   ▮ ▮ ▮ ▮ ▮ ▮ ▮     │
│   [ Moderate ]   │                         │                     │
│                  │                         │   65 dB             │
│  Cur · Avg · Peak│                         │   7-day average     │
├──────────────────┴─────────────────────────┴─────────────────────┤
│  Reference scale  ━━━━━╋━━━━╋━━━━━━━━╋━━━━╋━━━━╋━━━ 0—120 dB     │
└──────────────────────────────────────────────────────────────────┘
```

## Noise level thresholds

| dB range | Color    | Label      |
|----------|----------|------------|
| < 30     | #00E676  | Very quiet |
| 30–50    | #66BB6A  | Quiet      |
| 50–65    | #FF9100  | Moderate   |
| 65–80    | #FF7043  | Loud       |
| ≥ 80     | #FF5252  | Very loud  |

## Notes

dB readings are computed from the RMS of the time-domain audio buffer and shifted to roughly match SPL ranges. This is **not** a calibrated SPL meter — readings are useful as a relative indicator, not for lab-grade measurement.

Microphone audio never leaves the device; everything runs locally.

## License

MIT
