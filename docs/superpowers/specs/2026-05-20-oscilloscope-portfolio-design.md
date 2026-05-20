# Oscilloscope Portfolio — Design Spec
Date: 2026-05-20

## Overview

Replace the current template-style `index.html` with a portfolio that looks and behaves like a 4-channel digital oscilloscope instrument. The site IS the instrument — there is no traditional nav, no section scroll, no cards. The visitor interacts with a hardware panel to navigate content.

---

## Visual Design

### Instrument Chassis
- Full-viewport fixed layout — the oscilloscope fills the screen, no page scroll on desktop
- Dark metal housing background (`#1e1a16` gradient), slight bevel/inset feel
- Brand strip at top: `M · O · SCOPE  MODEL CPE-2026 · 4CH · PINK PHOSPHOR`
- BNC port row at bottom (decorative): CH1–CH4 + EXT, with serial number `SN: M.OUDEH-CPE2026`
- Outer box shadow gives depth against the body background

### CRT Screen
- Occupies ~75% of the horizontal space
- Background: near-black `#05100a`
- Grid overlay: fine pink grid lines (`rgba(212,130,154,0.04)`) with major divisions every 5 cells (`rgba(212,130,154,0.09)`)
- Phosphor glow: inner box-shadow vignette for CRT depth
- Active channel indicator strip along top of screen: `CH1 ID · CH2 SIGNAL · CH3 LOG · CH4 SPEC` — active one has `▶` prefix and full opacity
- Waveform SVG trace on every channel view — unique shape per channel
- Measurement readouts bar at bottom of screen (always visible, values change per channel)
- Content renders inside the screen between the indicator strip and readout bar

### Color Palette
- Phosphor: `rgba(212,130,154, …)` — her pink used at varying opacities for glow, traces, text
- Chassis: `#1e1a16` / `#2a2520` browns
- Screen bg: `#05100a`
- Accent: `rgba(169,139,196, …)` purple used sparingly (secondary traces, CH2 overlay)
- All text inside screen: pink phosphor family only — no white

### Typography
- Screen text: `'Courier New', monospace` — instrument readout feel
- Headings inside screen: `Georgia, serif` italic — personal touch within the technical frame
- Chassis labels: monospace, very small, muted

---

## Panel Layout

```
┌─────────────────────────────────────────────────────┐
│  M · O · SCOPE                    MODEL CPE-2026    │  ← brand strip
├──────────────────────────────────┬──────────────────┤
│                                  │  [ CHANNELS ]    │
│                                  │  ┌─────────────┐ │
│         CRT SCREEN               │  │ CH1  ID  ●  │ │  ← active LED
│                                  │  │ CH2  SIGNAL │ │
│  ┌ CH1 ID ·CH2 SIGNAL ·CH3· ┐   │  │ CH3  LOG    │ │
│  │                           │   │  │ CH4  SPEC   │ │
│  │   [active channel content]│   │  └─────────────┘ │
│  │                           │   │  [ TIMEBASE ]    │
│  │   waveform trace SVG      │   │   ○SEC  ○VOLTS   │
│  │                           │   │                  │
│  │   readouts: FREQ·V·STATUS │   │  [ ⬡ PROBE ]    │  ← contact
│  └───────────────────────────┘   │                  │
├──────────────────────────────────┴──────────────────┤
│  ○CH1  ○CH2  ○CH3  ○CH4  ○EXT        SN: M.OUDEH   │  ← BNC ports
└─────────────────────────────────────────────────────┘
```

---

## Channels (Content)

### CH1 — ID (About)
- Large italic serif name: "Mariam / Oudeh."
- Signal trace: steady sine-like wave
- Tagline: "Senior CPE · CSU Sacramento · Fall 2026"
- One-paragraph bio
- Inline skill tags styled as `[TAG]` channel annotations
- Readouts: `FREQ: 1.0 kHz coffee` · `V RMS: 3.3V` · `PERIOD: ∞ cat naps` · `STATUS: OPEN`

### CH2 — SIGNAL (Projects)
- List of projects displayed as "signal events" on a timeline trace
- Each project: name (serif), one-line description, stack tags as annotation labels
- Active/featured project highlighted with brighter trace amplitude
- Projects: Hydroponic Greenhouse, Wearable Glucose Monitor, BT RC Car, Mandelbrot Sim, Skystar, TRIO Records
- Readouts: `CH: 6 signals` · `PEAK: Capstone` · `TRIGGER: STM32` · `MODE: CONTINUOUS`

### CH3 — LOG (Experience/Background)
- Timeline rendered as a waveform with "event markers" — vertical tick marks at each entry
- Each entry: date range, role, place, short description
- Entries: B.S. CPE at CSUS, TRIO Outreach Specialist, A.S. ARC
- Readouts: `SPAN: 2022–2026` · `EVENTS: 3` · `ΔTIME: 4 yr` · `TREND: ↑`

### CH4 — SPEC (Skills)
- Styled like a datasheet or component spec table
- Groups: Embedded Systems, Hardware & PCB, Languages, Currently Exploring
- Each item is a `param: value` row in monospace
- Readouts: `PROTOCOLS: 5` · `LANGUAGES: 4` · `TOOLS: 6` · `V: 3.3/5V`

### PROBE button
- Opens a contact overlay on top of the screen
- Overlay styled as a "probe connection" panel — email, LinkedIn, GitHub, Resume
- Close button dismisses overlay

---

## Interactions

- **Channel switching**: clicking CH1–CH4 buttons fades out current screen content and fades in new channel content; LED on active button glows; channel indicator strip updates
- **PROBE button**: toggles contact overlay with fade transition
- **Knobs**: decorative only — no functional behavior needed
- **Hover on CH buttons**: subtle border brightening, no heavy animation
- **Transition**: screen content cross-fades (~300ms ease). Waveform SVG animates in (stroke-dashoffset draw effect) on channel switch
- **Cursor**: keep existing paw cursor — it's personal and works perfectly with the instrument aesthetic

---

## Mobile

The full-panel layout doesn't work below ~700px. Mobile gets a simplified version:
- Chassis frame collapses: screen takes full width, control panel moves to a horizontal tab bar at the bottom
- CH1–CH4 become icon+label tabs (bottom nav)
- PROBE becomes a floating button (bottom right)
- Screen content scrolls vertically within the screen area
- Grid and glow effects preserved

---

## Implementation

- Single `index.html` file (no build step, no framework — matches current setup)
- All CSS in `<style>` block, all JS in `<script>` block
- Content is static — no data files
- Replaces current `index.html` entirely
- Keep existing assets: `photo.jpg`, `Mariam_Oudeh_Resume.pdf`
- Keep existing font imports: Playfair Display, DM Mono, DM Sans (use DM Mono as the primary screen font; Playfair for serif headings inside the screen)
- Keep existing paw cursor SVG and animation

---

## Out of Scope

- No CSS animations beyond: cursor, channel fade transition, waveform draw, LED glow pulse
- No canvas/WebGL — SVG waveforms only
- No dark/light mode toggle
- No JS frameworks or build tools
