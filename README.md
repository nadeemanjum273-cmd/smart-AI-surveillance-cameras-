# smart-AI-surveillance-cameras-

## Chakki_AI: Modular Edge Surveillance & Compound Entity Tracking System

Production-grade edge computer vision system for rural farm and solar flour mill (Chakki) security monitoring. Powered by YOLOv8 custom trained weights, ByteTrack multi-object tracking, and virtual polygonal spatial zones.

---

## Features

- **Custom YOLOv8 Object Detection:** Trained for rural objects (`animal`, `animal_cart`, `car`, `grain_sack`, `motorcycle`, `object`, `person`).
- **ByteTrack Tracking:** Sustains track IDs across occlusions and tree foliage.
- **Calibrated Polygonal Zones (1080p):**
  - **Zone 1: Main Approach Road:** Driveway and village entrance path.
  - **Zone 2: Solar Chakki Door:** Flour mill receiving area directly in front of the mill entrance.
  - **Zone 3: Home & Rear Entrance:** Path behind the tree canopy leading to living quarters.
  - **Zone 4: Farm & Livestock Enclosure:** Animal yard and cart pen.
- **Ground-Contact Tracking:** Uses bounding box bottom-center point $( \frac{x_1 + x_2}{2}, y_2 )$ to prevent 2D perspective false triggers.
- **15-Frame Motion Vector Analysis:** Determines direction of movement (`IN` Ingress, `OUT` Egress, `STATIONARY`).
- **Compound Behavioral Logic:**
  - **Bike Ridership:** Detects Single Rider vs Double Sawari.
  - **Chakki Customers:** Detects persons or motorcycles with grain sacks near Zone 2.
  - **Farm Visits & Home Visits:** Counts pedestrians heading into zones.
  - **Ambiguity Filter:** Marks uncertain overlaps for review.
- **Deduplication:** 60-second cooldown per track ID prevents redundant alerts.
- **Snapshots & Manifest:** Auto-crops high-resolution thumbnails to `/snapshots` and logs events to CSV and JSON manifests.
- **Continuous Watch Mode:** Automatically processes new 1-minute clips as the camera saves them.

---

## Project Structure

```
├── configs/
│   ├── zones.json                   # 1080p Calibrated Virtual Polygonal Zones
│   └── reference_frame.jpg          # Reference frame for calibration
├── zones.py                         # Zone Manager & 15-frame motion vector
├── tracker.py                       # ByteTrack YOLOv8 integration
├── classifier.py                    # Compound entity & behavioral reasoning
├── pipeline_runner.py               # Ingestion, cooldown, snapshots & logger
├── calibrate_zones.py               # Interactive point-and-click zone tool
├── train.py                         # YOLOv8 custom training script
├── snapshots/                       # Auto-generated event thumbnails
└── logs/
    ├── events_manifest.csv          # Structured CSV event log
    └── events_manifest.json         # Structured JSON telemetry log
```

---

## Quickstart

### 1. Requirements Installation
```bash
pip install opencv-python ultralytics torch
```

### 2. Run Live Visualization
```bash
python pipeline_runner.py --input-dir "D:\xiaomi_camera_videos\94f8276acb06" --visualize
```
*Keyboard controls:*
- `Space`: Pause / Resume
- `q`: Skip to next video clip
- `ESC`: Exit

### 3. Continuous Camera Watch Mode
```bash
python pipeline_runner.py --input-dir "D:\xiaomi_camera_videos\94f8276acb06" --watch
```

### 4. Interactive Zone Calibration
```bash
python calibrate_zones.py
```
