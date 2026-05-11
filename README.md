# Lightweight RTA

Lightweight RTA is a browser-based real-time audio analyzer for live sound, tuning, and room checks.

Version: 1.4.0

## What It Does

- Captures audio from microphone or screen/tab audio.
- Renders real-time spectrum in three views:
  - 1/3-octave bars
  - 1/x/auto-octave bars
  - FFT line
- Supports log/linear frequency scaling, min/max frequency windowing, smoothing, and dB range presets.
- Includes style customization for bars, FFT line, grid, labels, and background gradients.
- Saves and loads bank presets (with import/export JSON support).
- Provides built-in tuning generators:
  - Pink noise
  - Sine tone (note or manual Hz)
  - Sine sweep
- Displays optional moving sweep marker on the RTA during sine sweep.

## Menus

- Source:
  - Mic input
  - Screen/Tab audio input
  - Stop
- Mode:
  - View, scale, frequency bounds, FFT size, tooltip mode
  - Smoothing and dB range
  - Peak hold and reset
- Style:
  - Color and gradient controls
  - Bar, FFT, grid, and background controls
- Presets:
  - Bank load/save/default reset
  - Bank import/export
  - Style presets
- Tuning:
  - Simultaneous generator mode
  - Master output level
  - Per-source levels when simultaneous mode is enabled
  - Pink noise toggle
  - Sine tone toggle with pitch mode:
    - Note mode via musical note selector
    - Hz mode via numeric input
  - Sine sweep toggle with start/end frequency and duration
  - Optional sweep marker rendering on analyzer
- Help:
  - Quick keyboard shortcuts
  - Log panel toggle

## Keyboard Shortcuts

- Space: freeze/resume
- 1 / 2 / 3: load bank 1 / 2 / 3
- Left / Right: adjust max Hz
- Up / Down: adjust min Hz
- L: cycle view
- F: freeze/resume
- R: reset peaks
- P: toggle peak hold
- G: toggle grid
- H: toggle Hz labels
- M: start mic
- T: start screen/tab capture
- S: stop

## Presets and Bank Files

Banks are stored in localStorage under `rta-bank-1`, `rta-bank-2`, and `rta-bank-3`.

Import/export format:

- Exported files include metadata:
  - `version`
  - `source` (`lightweight-rta`)
  - `bank`
  - `exportedAt`
  - `settings`
- Import accepts exported wrapped format and legacy plain settings object.
- Imported settings are normalized and clamped to safe ranges.

## Security and Hardening

This project includes multiple runtime and server-side hardening controls.

Client-side hardening:

- Strict settings normalization for imported/saved bank data.
- Enum validation and numeric clamping for key controls.
- Color format checks and boolean coercion.
- Import file size limit.
- Metadata checks for wrapped import format (`source`, `version`).

Server-side hardening (`.htaccess`):

- Directory listing disabled (`Options -Indexes`).
- Access blocked to `.git`, `.gitignore`, and hidden dotfiles.
- Security headers:
  - `X-Content-Type-Options: nosniff`
  - `X-Frame-Options: SAMEORIGIN`
  - `Referrer-Policy: strict-origin-when-cross-origin`

## File Layout

- `index.html`: primary app entry
- `dev.html`: development/alternate entry with same core functionality
- `.htaccess`: Apache access control and response security headers
- `favicon.ico`: icon

## Running

This is a static web app.

- Serve via Apache/Nginx or any static file server.
- HTTPS is recommended so media capture APIs behave consistently.
- Use modern Chromium-based browser or Firefox for best Web Audio support.

## Operational Notes

- Keep generator level low at startup to avoid feedback.
- When using live mic capture and speakers, enable outputs cautiously.
- On mobile portrait layouts, menu rows are positioned below wrapped main menu rows to avoid overlap.

## Versioning

Current app version is surfaced in:

- Page title (`Lightweight RTA v1.4.0`)
- Footer status text on analyzer canvas
- Startup log line (`Ready v1.4.0`)

Update `APP_VERSION` in both `index.html` and `dev.html` when releasing a new build.
