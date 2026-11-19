# Jeevan Pramaan — Plain-Language Smart Auto-Capture (Version 2)

A front-end functional prototype built for a **Design Thinking (Human-Centred Design Process)** assignment: **Experiment 3 (Ideation and Prototyping)**, Faculty of Engineering and Technology.

---

## 📌 Context: Persona Viswan (82)
The target user is **Viswan, an 82-year-old pensioner** living in Kerala, India, with limited digital experience and mild age-related hand tremors. Each November, he must submit his annual *Jeevan Pramaan* digital life certificate using biometric face authentication to continue receiving his pension.

In the current government application, any failure (dim lighting, camera shake, minor head tilt) produces only a generic, unhelpful error:
> **"Verification Failed. Please try again."**

This causes severe anxiety, repeated blind guessing, and ultimately forces elderly pensioners to travel to crowded treasury offices in person.

---

## 🎯 What Changed in Version 2 (Addressing Teacher Feedback)

In **Version 1**, the prototype only checked brightness when the user manually pressed "Take Photo", which still allowed users to capture blurry, shaky, or dark photos.

**Version 2** redesigns the interaction model entirely:
1. **Automatic Capture Only**: Manual photo-taking is eliminated during regular verification. The camera captures automatically only after **all 9 quality checks pass continuously for 1.5 seconds**.
2. **Visual Countdown Ring**: An animated SVG progress ring around the face oval indicates the 1.5-second hold. If *any* check fails during the countdown, it immediately resets to 0.
3. **"Help Me" Button**: In place of a manual shutter button, a prominent **"Help me — What is wrong?"** button (64px+ tall) opens an accessible modal with an enlarged explanation and actionable steps.
4. **Real Browser Face Detection**: Powered by **MediaPipe Tasks Vision** (`FaceLandmarker` loaded from jsDelivr with pinned version `0.10.14`), with automatic fallback to the native browser `FaceDetector` API.
5. **Continuous 10 Hz Frame Evaluation**: Runs 9 quality checks approximately 10 times per second directly on the client canvas.
6. **Strict Priority Plain-Language Guidance**: Single instruction displayed at a time using Class 5 reading level, 20px+ font, high contrast, and non-color-only indicators.
7. **Anti-Flicker Protection**: Instructions stay visible for at least 700 ms before updating to avoid cognitive overload.
8. **Optional Voice Guidance**: Browser `speechSynthesis` speaks instructions clearly at a comfortable pace with a 4-second anti-repetition cooldown.
9. **Final Strict Still Verification**: Evaluates the captured still frame before final certificate submission, displaying measured lighting, sharpness, and stability metrics.
10. **Teacher Demo Controls**: Includes a live 9-check pass/fail inspection checklist and a simulation dropdown for offline demonstration without a camera.
11. **Session Attempt History Log**: Automatically records every try with timestamp, outcome, and reason.
12. **High-Contrast Themes**: Full support for Light and Dark themes using Atkinson Hyperlegible typography.

---

## 🔬 How Each of the 9 Checks Works

The checks are evaluated in strict priority order so the user receives the single most helpful instruction:

| Priority | Check Name | Detection Method | Threshold | Plain-Language Message |
|:---:|:---|:---|:---|:---|
| **1** | **Face Present** | MediaPipe landmark count | Exactly 1 face required | *"Looking for your face…"* / *"Only one person allowed"* |
| **2** | **Face Size** | Bounding box height relative to video height | 28% – 65% of frame | *"Move a little closer"* / *"Move a little farther back"* |
| **3** | **Face Position** | Face center relative to oval center `(0.50, 0.45)` | Horizontal ±14%, Vertical ±15% | *"Move phone a little left / right / up / down"* |
| **4** | **Face Straight** | Eye-line angle (roll) and nose-cheek ratio (yaw) | Roll $\le 11^\circ$, Yaw offset $\le 0.14$ | *"Keep your head level"* / *"Look straight at the screen"* |
| **5** | **Eyes Open** | Eye Aspect Ratio (EAR) from eyelid landmarks | Average EAR $\ge 0.16$ | *"Please open your eyes"* |
| **6** | **Brightness** | Mean luminance ($0.299R + 0.587G + 0.114B$) on face crop | 65 – 215 (out of 255) | *"Photo too dark. Move near a window"* / *"Too much bright light"* |
| **7** | **Glare** | Percentage of face crop pixels $\ge 245$ luminance | $\le 8.0\%$ blown-out pixels | *"Too much glare on face. Turn away from bright light"* |
| **8** | **Sharpness** | Laplacian variance on $100 \times 100$ face crop | Score $\ge 45.0$ | *"Photo is blurry. Wipe your camera lens and hold steady"* |
| **9** | **Stability (Shaky)** | Max drift across 9 frames + mean frame-to-frame pixel diff | Drift $\le 3.8\%$, Diff $\le 14.5$ | *"Hold the phone still"* (immediately resets countdown) |

---

## 🎛️ How to Tune Thresholds (`CONFIG`)

All thresholds are isolated inside the `CONFIG` object at the top of the `<script>` block in `index.html`. The object is also exposed to the browser console via `window.CONFIG` so you can tune values live during class:

```javascript
// Example: Open DevTools Console (F12) and type:
window.CONFIG.COUNTDOWN_DURATION_MS = 2000;   // Change countdown to 2.0 seconds
window.CONFIG.MIN_FACE_BRIGHTNESS = 50;        // Relax dark room threshold
window.CONFIG.MIN_SHARPNESS_LAPLACIAN = 35.0;  // Relax blur threshold for older webcams
```

Key configurable fields in `index.html`:
- `COUNTDOWN_DURATION_MS`: Duration (ms) all checks must hold (default: `1500`)
- `FACE_SIZE_MIN` / `FACE_SIZE_MAX`: Face height boundaries (default: `0.28` and `0.65`)
- `MAX_HEAD_ROLL_DEG`: Maximum head tilt in degrees (default: `11.0`)
- `MIN_FACE_BRIGHTNESS` / `MAX_FACE_BRIGHTNESS`: Luminance range (default: `65` – `215`)
- `MIN_SHARPNESS_LAPLACIAN`: Blur cutoff score (default: `45.0`)
- `MAX_FRAME_DIFF_MEAN`: Pixel movement cutoff for shake rejection (default: `14.5`)

---

## 🚀 Running the Project

Because browser camera APIs (`navigator.mediaDevices.getUserMedia`) require a secure context (`localhost` or HTTPS):

### Running Locally
Run Python's built-in web server in the project folder:
```bash
# In Windows PowerShell / Command Prompt / Terminal:
python -m http.server 8000
```
Then visit:
```text
http://localhost:8000
```

### Enabling on GitHub Pages
1. Push this repository to GitHub.
2. In your GitHub repository, click **Settings** > **Pages** (in the left sidebar).
3. Under **Branch**, select `main` (or `master`) and folder `/(root)`.
4. Click **Save**.
5. Your live prototype will be accessible over HTTPS (e.g. `https://<username>.github.io/<repo>/`).

---

## 🧪 Acceptance Test Procedures

| Test Case | Procedure | Expected Behavior |
|:---|:---|:---|
| **1. Dim room / Covered light** | Dim room lighting or block ambient light. | Countdown will not start; instruction reads *"Photo too dark. Move near a window."* |
| **2. Shaky phone or laptop** | Gently shake or move the device while framed. | Countdown immediately resets to 0; instruction reads *"Hold the phone still."* |
| **3. Turn head away / Tilt** | Turn head $30^\circ$ sideways or tilt head ear-to-shoulder. | Instruction reads *"Look straight at the screen"* or *"Keep your head level."* |
| **4. Perfect steady hold** | Keep face centered, level, well-lit, and steady. | Green ring smoothly fills over 1.5 s; auto-captures with *"Certificate Submitted!"* |
| **5. Two people in frame** | Have a second person lean into the camera frame. | Detection immediately stops countdown; instruction reads *"Only one person allowed."* |
| **6. Distance extremes** | Lean very close (<20 cm) or sit far back (>1.5 m). | Prompts *"Move a little farther back"* or *"Move a little closer."* |
| **7. Old-version comparison** | Turn on the *"Compare with today's version"* toggle. | Live guidance disappears; manual shutter appears; clicking rejects with generic *"Verification Failed"* and zero explanation. |

---

## ⚠️ Known Limitations & Disclaimers

1. **Academic Demonstration Only**: This prototype is built strictly for educational design thinking research. It is **not connected** to the official UIDAI or Jeevan Pramaan backend and does not communicate with government servers.
2. **Webcam vs. Hardware IR**: The real government app on Android uses UIDAI Aadhaar FaceRD, which may leverage dedicated infrared sensors and depth hardware. Our web prototype runs client-side RGB computer vision.
3. **Lighting Hardware Variability**: Cheap external webcams with aggressive auto-exposure may take 1–2 seconds to stabilize lighting when the camera starts.
4. **Zero Data Retention**: All video processing and landmark detection occur exclusively inside browser memory. No images or facial metrics are stored or transmitted anywhere.
