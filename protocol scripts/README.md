# RAISE-Vision Workflow

Automated sessile drop contact angle measurement workflow for the Opentrons OT-2,
using a custom stage, drypad apparatus, and camera for image capture.

## Overview

This folder contains the full RAISE-Vision measurement pipeline, split across two parts:

1. **`RAISE_Vision.ipynb`** — runs on the OT-2, controls the physical measurement cycle.
2. **`RAISE-Vision Integration Code/`** — runs on the host PC, handles the camera, the
   client/server link to the OT-2, and image analysis.

### 1. OT-2 protocol (`RAISE_Vision.ipynb`)

1. Homes the OT-2 and loads custom labware (stage, drypad rack, trash).
2. Dries all four sides of the lens mount using a rolled drying insert.
3. Waits for a fixed air-exposure period before droplet application.
4. Performs 10 sessile drop cycles: applies a 5 µL droplet to the lens,
   captures an image, then removes the droplet with a drypad tip.
5. Sends the captured images to the camera server for analysis.

### 2. Camera & analysis code (`RAISE-Vision Integration Code/`)

- **`Server.py`** — base TCP socket server class. Opens a connection on the host
  PC and accepts commands sent from the OT-2 notebook.
- **`CameraServer.py`** — subclasses `Server`; listens for commands
  (`NEW-FOLDER`, `TAKE-IMAGE`, `ANALYZE-IMAGES`) sent from the OT-2 and
  dispatches them to the camera or the image analysis pipeline.
- **`CameraDevice.py`** — wraps the USB camera (via OpenCV) with capture,
  live preview (Tkinter window), and image-saving functionality.
- **`ADC.py`** — small abstract base classes (`Device`, `ReadableDevice`,
  `WritableDevice`) that `CameraDevice` is built on. No changes needed here.
- **`CurvedSurface_CA_ImageProcessing.py`** / **`.ipynb`** — image processing
  pipeline that detects the droplet contour on the curved lens surface and
  computes left/right contact angles for each image. Exposes `analyze_run()`,
  which is called automatically by `CameraServer` once a run completes, and
  writes `curve_equations.txt` and `data.csv` to a `Processed/` subfolder.

## Prerequisites

- `CameraServer.py` must be running on the Anaconda Prompt *before* the OT-2
  notebook is executed, the OT-2 notebook's `Camera` class connects to it
  over a raw socket.
- OT-2 must be reachable, and the custom labware definitions
  (`raise_vision_drypad_tiprack`, `raise_vision_stage`, `raise_vision_trash`)
  must be installed — see the `labware/` and `JSON Files/` folders in this repo.
- Deck layout:
  - Slot 1: `raise_vision_stage`
  - Slot 5: `raise_vision_trash`
  - Slot 6: `raise_vision_drypad_tiprack`
  - Slot 8: `corning_24_wellplate_3.4ml_flat` (test liquid)
  - Slot 10: `opentrons_96_tiprack_20ul` (drying inserts)
  - Slot 11: `opentrons_96_tiprack_20ul` (droplet tips)
- Camera PC needs Python 3.10 with `opencv`, `pillow`, `scikit-image`,
  `pandas`, `matplotlib`, and `tqdm`, see [Environment setup](#environment-setup) below.

## Environment setup

Two conda environment files are provided for the camera/analysis PC (`RAISE-Vision Integration Code/`),
depending on OS.: _(If one version does not work, try the other even if it doesn't match the OS.)_

```bash
# macOS
conda env create -f RAISE-Vision.yml

# Windows
conda env create -f RAISE-Vision.yaml

conda activate RAISE-Vision
```

## Usage

1. Place the lens and lens mount on the stage.
2. On the camera PC, activate the `RAISE-Vision` environment and start the
   camera server:
```bash
   python CameraServer.py
```
3. In `RAISE_Vision.ipynb`, update `IP_ADDRESS` and `PORT` to match the camera
   server (printed to console on startup).
4. Update `run_dir` to a new, descriptive folder name for the current test
   (e.g. `../YYYYMMDD-material-config-RV-RT/`).
5. Run all cells in order. The `dryNum` counter tracks which drying insert
   well to use next and increments automatically — reset it to 0 when
   loading a fresh tip rack.
6. Images are saved to `run_dir` and analyzed automatically at the end of the
   run via `analyze_run()`, which writes `curve_equations.txt` and `data.csv`
   to a `Processed/` subfolder inside `run_dir`.
7. Call `cam.close()` once all trials for the session are complete.

## Notes

- Each drying/droplet cycle uses fresh disposable tips (drying inserts and
  drypad tips are single-use, discarded after each application).
- The sessile drop cycle runs 10 times per lens by default; adjust the
  `range(10)` loop in the main workflow cell to change this.
- In `CurvedSurface_CA_ImageProcessing.py`, the `experiment` variable inside
  `analyze_run()` must be set to match the run type (`"T&RH"` or `"B&R"`),
  and `img_crop_values` may need adjusting per camera setup — see
  `test_droplet_img()` / `test_image_size()` for calibration.
