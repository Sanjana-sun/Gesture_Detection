# Gesture Detection: air-drawing with hand tracking

Draw on screen by moving your finger in the air. A webcam tracks your hand in real time, and finger gestures select colors, draw strokes, and clear the canvas, with no mouse or touchscreen. It is a hands-free input demo aimed at human-computer interaction and accessibility.

## What it does

`gest.py` opens your webcam and tracks a single hand. Your index fingertip is the pen: move it below the toolbar to draw, and move it up to the toolbar to pick a color or clear. Strokes are rendered both on the live camera feed and on a separate white paint canvas. Specific finger poses trigger actions such as starting a new stroke or saving the canvas.

## Tech stack

- Python
- MediaPipe Hands for real-time hand landmark detection (single hand, detection confidence 0.7)
- OpenCV for webcam capture, drawing, the toolbar UI, and window display
- NumPy and `collections.deque` for the stroke buffers

## How it works

- MediaPipe returns 21 hand landmarks per frame. The code scales them to pixel coordinates and uses the index fingertip (landmark 8) as the cursor and the thumb tip (landmark 4) as a gesture reference.
- Strokes are stored as deques of points, one set of buffers per color (blue, green, red, yellow). A new deque is pushed whenever the pen lifts, so separate strokes do not connect.
- A toolbar drawn across the top of the frame defines hit regions: CLEAR resets all buffers and wipes the canvas, and the four color regions switch the active color.
- The gap between the thumb and index fingertip acts as a pen-up gesture: when they come close together the app starts a fresh stroke instead of drawing.
- When all non-thumb landmarks fold and the thumb sits left of the index (a thumbs-up style pose), the current canvas is written to `thumbs_up_canvas.png`.
- Each frame redraws every stored stroke onto both the camera feed and the paint canvas.

This repo contains the real-time hand-tracking drawing application. It uses MediaPipe's pretrained hand-landmark model rather than a custom-trained network.

## Recognition and awards

This work placed 1st at TEJAS 2K24 and is related to the author's publication "Gesture-Based Control System" (journaleims.com, 2024). The publication reported 95% control accuracy for the gesture control system. That figure comes from the publication and describes the broader system evaluated there, not a benchmark of this specific script.

## How to run

```bash
pip install opencv-python mediapipe numpy
python gest.py
```

A webcam is required. Press `q` to quit.
