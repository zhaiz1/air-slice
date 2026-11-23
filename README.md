# Air Slice

**Slice fruit with your bare hands!**

A browser game where your index finger is the blade. Swing it in front of your webcam to slice flying fruit and dodge the bombs. Hand tracking runs entirely in the browser, and no video is ever uploaded.

## How to play

- Allow camera access, then swipe your index finger through one of the melons to pick a difficulty (Easy / Normal / Hard).
- Only fast swipes cut. Slow movements don't. Hitting a bomb or missing 3 fruits ends the game.
- Hold a fist for half a second to pause, and again to resume. You can also press P or Space.
- Switch between English and Chinese with the button in the bottom-right corner, or press L.
- No camera? Play with a mouse or touchscreen.

## Run locally

Browsers only allow camera access on `https` or `localhost`:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## How it works

- **Hand tracking**: MediaPipe Hand Landmarker detects 21 keypoints per hand. Landmark 8 (the index fingertip) is the blade, and up to two hands are tracked at once.
- **Coordinate mapping**: Normalized landmarks are mapped to screen space using the same crop that fits the video to the screen, and the x axis is mirrored.
- **Jitter removal**: A One Euro filter smooths heavily when the hand is still and barely at all during fast swipes.
- **Slice detection**: The segment between the fingertip's last two positions is tested against each fruit, so fast swipes can't skip over a fruit between frames. A cut only counts above a speed threshold.
- **Fist detection**: A hand counts as a fist when all four fingertips are closer to the wrist than their middle joints. Holding it for 0.5 s toggles pause.
- Rendering uses Canvas 2D, and sound effects are synthesized live with Web Audio. No external assets.

Built for CIS 5810 Computer Vision.
