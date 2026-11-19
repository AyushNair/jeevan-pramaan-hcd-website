# Changelog — Jeevan Pramaan Prototype

All notable changes from Version 1 to Version 2 of the front-end prototype.

---

## [Version 2.0] — Smart Auto-Capture & Live Plain-Language Guidance

### Added
- **Smart Auto-Capture**: Removed manual photo capture as the primary mechanism. The system continuously evaluates incoming frames and captures automatically only when all 9 quality checks pass for 1.5 continuous seconds.
- **Visual Countdown Progress Ring**: Added an animated SVG circular/elliptical progress ring and floating badge indicating the 1.5-second hold. If any check fails, the ring resets instantly.
- **"Help Me — What is wrong?" Button**: Added an accessible button (min 64px tall) replacing the manual shutter button. Tapping it opens a modal displaying the current highest-priority issue in 26px+ plain-language text with actionable instructions.
- **MediaPipe Tasks Vision Integration**: Integrated Google MediaPipe Tasks Vision (`FaceLandmarker` v0.10.14) running client-side WebAssembly with GPU/CPU delegates and fallback to browser native `FaceDetector`.
- **9 Continuous Quality Checks (10 Hz)**:
  1. *Face Present*: Validates exactly one face (rejects 0 faces and multi-person frames).
  2. *Face Size*: Rejects faces too far (<28% frame height) or too close (>65% frame height).
  3. *Face Centering*: Measures displacement from on-screen oval and guides left/right/up/down.
  4. *Face Straight*: Measures head roll (tilt) and yaw (turn) angles via landmarks.
  5. *Eyes Open*: Measures Eye Aspect Ratio (EAR) from upper/lower eyelid landmarks.
  6. *Face Brightness*: Measures mean luminance directly on the face bounding box crop.
  7. *Glare Detection*: Detects blown-out pixel percentage on the face region.
  8. *Sharpness (Laplacian Variance)*: Evaluates blur using a discrete Laplacian kernel.
  9. *Stability (Shaky Check)*: Tracks face bounding-box drift over 9 frames and computes mean frame-to-frame pixel differences.
- **Anti-Flicker Instruction Engine**: Enforced a minimum 700 ms display duration for instruction messages to prevent rapid visual oscillation and anxiety for elderly users.
- **Browser Voice Guidance (`speechSynthesis`)**: Spoken instructions with a 4-second anti-repetition cooldown, announcing status changes, "Photo captured", and "Certificate submitted".
- **Final Strict Still Verification**: Runs a post-capture validation check on the captured still image before displaying the certificate confirmation.
- **Teacher & Demo Controls Panel**:
  - Live 9-check inspection checklist displaying real-time pass/fail states and live metrics (luminance %, sharpness score, drift px).
  - "Force Result" simulation dropdown for classroom testing without a webcam.
- **Session Attempt History Log**: Side-panel table recording each attempt number, timestamp, outcome, and specific reason.
- **Light & Dark Theme Toggle**: Added high-contrast dark theme option with system `prefers-color-scheme` support.
- **Accessible Typography**: Implemented Atkinson Hyperlegible font designed specifically for low-vision readers.

### Changed
- **Shutter Button Removal**: The manual shutter button is hidden during regular capture mode and appears only when the user toggles "Compare with today's version".
- **Old Version Comparison**: Updated the comparison toggle so that in "Today's Version" mode, all live guidance is turned off, any frame is accepted on shutter click, and the app rejects with the generic "Verification Failed" message and zero explanation.
- **Responsive Layout**: Replaced the single-column layout with a 2-column desktop / stacked mobile layout pairing the phone mockup with the testing and research side panel.
- **Configurability**: Consolidated all detection thresholds into a documented `CONFIG` object accessible via `window.CONFIG`.

---

## [Version 1.0] — Initial Lo-Fi Functional Prototype

### Initial Features
- Static lo-fi phone mockup card.
- Basic webcam brightness measurement upon clicking a manual "Take Photo" button.
- Three hardcoded/sample rejection reasons (Photo too dark, Too much glare, Face not fully visible).
- Manual "Compare with old version" toggle.
- Expandable "Viswan's experience" empathy block (Says, Thinks, Feels, Does).
