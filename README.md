# IDUN Controller

A head-controlled cursor built with the IDUN Guardian 3 EEG/IMU earbuds and the idun-guardian-sdk. This project uses real-time gyroscope data streamed from in-ear EEG hardware to control mouse cursor movement, with jaw clench detection for clicking on supported account tiers.

What It Does
Streams live IMU (gyro x/y) and EEG data from IDUN Guardian 3 earbuds over WebSocket
Translates head pitch and yaw into relative cursor movement via pyautogui
Applies dead zone filtering and exponential smoothing to reduce jitter
Reads EEG signal quality scores in real time and gates cursor activity when signal is poor
Supports jaw clench click detection (requires device classifier entitlement on your IDUN account)
Falls back to keyboard click trigger when jaw clench is not available on your tier
Current State

The data pipeline works consistently — IMU data streams reliably, quality scoring fires every ~1.5 seconds, and the cursor responds to head movement in real time. Jaw clench detection works on supported accounts but requires specific deliberate head positioning to maintain signal quality above threshold.

Known issues still being worked on:

Cursor movement direction is inverted on one or both axes depending on headset orientation
Cursor accuracy needs improvement — small unintentional head movements cause drift
Gyro bias offset causes slow directional drift when holding still, partially mitigated by a one-shot calibration at startup
Running the SDK stream concurrently with the cursor loop in Jupyter requires a background thread workaround due to async event loop conflicts
Requirements
idun-guardian-sdk
pyautogui
keyboard
numpy
matplotlib

Install with:

bash
pip install pyautogui numpy matplotlib keyboard idun-guardian-sdk
Setup
Charge your IDUN Guardian 3 earbuds and ensure they are paired
Add your API token to Cell 2
Run cells in order: 1 → 2 → 3 → 5 → 6
Cell 5 must be running before Cell 6 — it starts the data stream that populates the shared state the cursor loop reads from
To stop the cursor loop at any time, run stop_cursor.set() in a new cell
Hardware

IDUN Guardian 3 in-ear EEG earbuds. Signal quality is sensitive to fit — repositioning the earbuds is often necessary to get quality scores above 50, which is the threshold for reliable cursor control.

Roadmap
Fix axis inversion based on headset orientation
Add proper multi-second gyro bias calibration instead of one-shot offset
Improve drift suppression during stillness
Explore velocity-based scaling so small movements are precise and large movements are fast
Investigate using EEG signal features beyond quality score for additional control inputs
Notes

JAW_CLENCH detection requires an active device classifier entitlement on your IDUN account. If you see Active device classifier entitlement required for: JAW_CLENCH in your output, your current tier does not include it and the keyboard fallback will be used instead.
