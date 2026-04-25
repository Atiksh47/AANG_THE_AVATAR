# 降神者 — Avatar: The Last Airbender

An interactive hand-tracking particle experience built with Three.js and MediaPipe. Control the four elements in real time using your webcam and hand gestures — just like Aang.

---

## Demo

Open `index.html` in a browser, allow webcam access, and bend.

---

## Gestures

| Gesture | Element | Visual Effect |
|---|---|---|
| ☝️ Index finger only | **Earthbending** | 7 jagged rock pillars erupt from the ground, slow heavy movement |
| ✌️ Two fingers (peace) | **Waterbending** | 5 sinusoidal wave rings spiral outward from a glowing blue orb |
| 🤟 Three fingers | **Firebending** | 5 twisted flame arms shoot upward, fading red → orange → ember sparks |
| 🖐️ Open palm | **Airbending** | Hourglass-shaped tornado cyclone with 4 fast-spinning spiral arms |
| 🤏 Pinch | **Avatar State** | All four elements erupt in 8 spiraling arms around a blazing white core |
| Neutral | **Neutral** | A soft blue-white avatar energy core |

---

## How It Works

### Stack
- **[Three.js](https://threejs.org/) r160** — 3D particle rendering via `THREE.Points` with additive blending
- **[MediaPipe Hands](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker)** — real-time hand landmark detection (21 keypoints per hand)
- **`UnrealBloomPass`** — HDR bloom glow, strength varies per element
- No build step — single HTML file, all dependencies via CDN

### Particle System
20,000 particles share three typed arrays (`position`, `color`, `size`). Each element defines a target configuration; every animation frame lerps the current arrays toward the targets:

```js
pos[i] += (targetPos[i] - pos[i]) * lerpSpeed;
```

Earth uses `lerpSpeed = 0.05` (sluggish, heavy). Air uses `0.12` (snappy). This gives each element a distinct physical character without any physics engine.

### Gesture Detection
Finger state is determined by comparing tip landmark Y to PIP joint Y — lower Y on screen means the finger is extended:

```js
const up = (tip, pip) => lm[tip].y < lm[pip].y;
```

Priority order: pinch → all 4 up → 3 up → 2 up → 1 up.

---

## Running Locally

No install required. Just open the file:

```bash
# Option 1 — direct file open
open index.html    # macOS
start index.html   # Windows

# Option 2 — local server (recommended for some browsers)
npx serve .
# or
python -m http.server 8080
```

> **Note:** Chrome and Edge work best. A webcam is required.

---

## Project Structure

```
AANG Airbender/
└── index.html    # Everything — scene, particle systems, hand tracking, UI
└── README.md
```

---

## Inspired By

- Avatar: The Last Airbender (Nickelodeon)
- JJK Cursed Technique particle demo (original concept this was built from)
