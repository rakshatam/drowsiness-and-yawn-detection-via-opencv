# Real-time Drowsiness and Yawn Detection System

[![Python 3.x](https://img.shields.io/badge/python-3.x-blue.svg)](https://www.python.org/downloads/)
[![OpenCV](https://img.shields.io/badge/OpenCV-2A9960?style=flat&logo=opencv&logoColor=white)](https://opencv.org/)
[![dlib](https://img.shields.io/badge/dlib-green.svg)](http://dlib.net/)
[![Scikit-learn](https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=flat&logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)

This project implements a real-time system to monitor a user's face via webcam and detect signs of drowsiness (closed eyes) and yawning. When detected, it triggers audible alerts to help prevent accidents, especially useful in driving or prolonged task scenarios. The system uses facial landmarks to calculate key indicators and provides visual feedback on the video feed.

---

## How It Works

The system operates by continuously analyzing video frames from a webcam, performing the following steps:

1.  ### **Facial Landmark Detection**
    * **Face Detection:** It first uses a **Haar Cascade classifier (`haarcascade_frontalface_default.xml`)** to quickly detect faces in the grayscale video frame. This method is faster but less accurate than dlib's default detector (which is commented out in the code).
    * **Landmark Prediction:** Once a face is detected, **dlib's `shape_predictor_68_face_landmarks.dat`** is used to pinpoint 68 specific facial landmarks (points on eyes, mouth, nose, jawline, etc.).

2.  ### **Drowsiness Detection (Eye Aspect Ratio - EAR)**
    * The **Eye Aspect Ratio (EAR)** is calculated for both the left and right eyes. EAR is a ratio of distances between the vertical eye landmarks and the horizontal eye landmarks.
    * When a person's eyes close, their EAR value drops significantly.
    * If the EAR falls below a predefined `EYE_AR_THRESH` (e.g., 0.3) for a sustained number of consecutive frames (`EYE_AR_CONSEC_FRAMES`, e.g., 30 frames), a **"DROWSINESS ALERT!"** is displayed on screen, and an audible "wake up sir" alarm is triggered.

3.  ### **Yawn Detection (Lip Distance)**
    * The vertical distance between the top and bottom lips is calculated using their respective facial landmarks.
    * When a person yawns, this distance increases.
    * If the calculated lip distance exceeds a `YAWN_THRESH` (e.g., 20), a **"Yawn Alert"** is displayed, and an audible "take some fresh air sir" alarm is triggered.

4.  ### **Alerting System**
    * Audible alarms are played using the `espeak` command-line tool.
    * These alarms run in separate **Python threads** (`threading.Thread`) to ensure they do not block the main video processing loop, maintaining real-time performance.

---
