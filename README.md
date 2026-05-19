# Curva

Per-pedal response curves and adaptive damping for sim racing on Windows.

[Download the latest release](https://github.com/FloreKoen/curva/releases/latest)

## What it does

You shape each pedal's response with its own curve, and you damp out the sensor noise around the resting position so the car stops twitching on the grid. Throttle, brake, clutch — each one gets its own settings.

The app sees your real pedals through HID, runs them through your curves and damping, and feeds the result into a virtual joystick that games pick up as normal hardware. Your real pedal device is hidden from games while Curva is active, so there is no double-input.

## The curve editor

You drag control points around a graph. Input on the X axis, output on the Y axis, both 0..100%. Segments between points are either:

- **Linear** — a straight line.
- **Smooth** — a monotonic cubic, the kind that cannot overshoot. Good for brake bite or throttle off-zone shaping where you do not want the curve to dip below or rise above the points you set.

Each segment picks its own kind. You can mix them.

There is no "curve type" dropdown with gamma / Bezier / piecewise / etc. Pedals are not a place where a curve should be allowed to overshoot the points you placed, so the math is fixed.

## Damping

A small adaptive low-pass filter sits in front of each curve. You give it a strength from 0 to 100% per direction (press / release) per pedal. It barely touches deliberate motion but it stops the resting jitter that sensors throw off when you take your foot off.

It runs **before** the curve, on raw input, so a steep curve cannot amplify pedal noise into a twitchy output.

## Activation

Curva has two states: inactive (real pedals visible to games, nothing curved) and active (real pedals hidden, virtual joystick live with your curves). One toggle switches between them. The bottom bar always shows which state you are in, and there is an orange stripe at the very top of the window when activation is on so you can never miss it.

Ctrl+Space toggles activation from anywhere inside the window.

If anything crashes while activation is on, the next launch detects it and offers to restore your real pedals. There is also a `restore.bat` in the install folder that unhides everything in one click, in case the app cannot start at all.

## Profiles

You can have multiple profiles per pedal device — one for ovals, one for road, one for rallying, whatever. Switching profiles is a click. Profiles save as JSON files under `%APPDATA%`, so you can back them up or move them between machines by copying.

## Install

Download the installer from the release page and run it.

The installer is a bootstrapper. It checks whether the two Windows drivers Curva needs are already on the machine, and only installs the ones that are missing:

- **HidHide** by Nefarius Software Solutions — a kernel driver that hides your real pedal device from games while Curva is active. MIT licensed.
- **vJoy Revived** by njz3 (maintained fork of the original by Shaul Eizikovich) — a virtual joystick driver that exposes the curved output to games. MIT licensed.

Both drivers are open-source and installable independently. Curva does not bundle them as redistributables — it chains their upstream MSIs so you get the same drivers their projects ship.

## Requirements

Windows 10 21H2 or later, 64-bit. A USB pedal set that shows up as a standard HID device.

## Quick start

1. Run the installer. Accept the driver installs if prompted.
2. Open Curva.
3. Pick your pedal device on the Devices tab. Run the setup wizard so Curva learns the axis for each pedal.
4. Switch to the Configure tab and shape the curves. Drag points around, right-click a segment to flip it between Linear and Smooth.
5. Hit Start in the bottom bar. Real pedals get hidden, virtual joystick goes live.
6. Map the virtual joystick axes in your game. They will read as "vJoy Device" on the standard joystick screen.

## Privacy

No network requests. Profiles and settings live in `%APPDATA%\Curva\`. Copy that folder to back things up.

## Source

Source code lives in a separate private repository. Builds here are produced from it.

## Made by

[kfsystems.eu](https://kfsystems.eu/)
