# Jeevan Pramaan — Human-Centred Design Website & Prototype

A comprehensive multi-page website and functional prototype built for a **Design Thinking (Human-Centred Design Process)** assignment: **Experiment 3 (Ideation and Prototyping)**, Faculty of Engineering and Technology.

---

## 📌 Project Overview

**Jeevan Pramaan** is India's biometric digital life certificate system for pensioners. Currently, when photo verification fails, elderly pensioners (such as persona Viswan, age 82) receive a generic, unhelpful error message: *"Verification Failed. Please try again."*

This project documents the entire human-centred design process to solve this issue and implements **Idea #1 from the Idea Selection Matrix**: replacing the generic failure message with specific, plain-language rejection reasons and single actionable next steps.

---

## 🌐 Website Structure

- **`index.html`** — **Home / Landing Page**: Overview of the project, mission, key statistics, and navigation.
- **`problem.html`** — **The Problem**: Detailed breakdown of the pain points elderly users face with the current system.
- **`research.html`** — **User Research**: Persona profile (Viswan, 82), 4-quadrant Empathy Map (Says, Thinks, Feels, Does), and research insights.
- **`ideation.html`** — **Ideation & Solutions**: How-Might-We statement, 5 evaluated concepts, Idea Selection Matrix, and design rationale.
- **`prototype.html`** — **Live Interactive Prototype**: Fully functional mobile simulation featuring:
  - Real camera integration with face alignment oval
  - Real-time ambient lighting/glare detection via HTML5 Canvas
  - Contextual error states ("Photo too dark", "Too much glare", "Face not fully visible")
  - "Compare with old version" side-by-side toggle
  - Expandable empathy panel
- **`style.css`** — Shared accessible stylesheet (warm neutral palette, large tap targets, high contrast, mobile responsive).

---

## 🚀 How to Run

Because the camera requires a secure context (`localhost` or HTTPS):

### Option 1: Python HTTP Server (Recommended)
```bash
python -m http.server 8000
```
Then visit: [`http://localhost:8000`](http://localhost:8000)

### Option 2: Node.js (npx serve)
```bash
npx serve .
```

---

## 👥 Course & Assignment Context
- **Course**: Human-Centred Design Process
- **Experiment**: Experiment 3 — Ideation and Prototyping
- **Faculty**: Faculty of Engineering and Technology
