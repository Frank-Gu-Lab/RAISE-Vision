# RAISE-Vision Workflow

Automated sessile drop contact angle measurement workflow for the Opentrons OT-2, 
using a custom stage, drypad apparatus, and camera for image capture.

## Overview

This notebook runs a full RAISE-Vision measurement cycle:

1. Homes the OT-2 and loads custom labware (stage, drypad rack, trash).
2. Dries all four sides of the lens mount using a rolled drying insert.
3. Waits for a fixed air-exposure period before droplet application.
4. Performs 10 sessile drop cycles: applies a 5 µL droplet to the lens, 
   captures an image, then removes the droplet with a drypad tip.
5. Sends the captured images to the camera server for analysis.

## Prerequisites

- **CameraServer.py must be running** on the Anaconda Prompt *before* this 
  notebook is executed, the `Camera` class connects to it over a raw socket.
- OT-2 must be reachable and the custom labware definitions 
  (`raise_vision_drypad_tiprack`, `raise_vision_stage`, `raise_vision_trash`) 
  must be installed, see the `labware/` and `JSON Files/` folders in this repo.
- Deck layout:
  - Slot 1: `raise_vision_stage`
  - Slot 5: `raise_vision_trash`
  - Slot 6: `raise_vision_drypad_tiprack`
  - Slot 8: `corning_24_wellplate_3.4ml_flat` (test liquid)
  - Slot 10: `opentrons_96_tiprack_20ul` (drying inserts)
  - Slot 11: `opentrons_96_tiprack_20ul` (droplet tips)

## Usage

1. Place the lens and lens mount on the stage.
2. Update `IP_ADDRESS` and `PORT` to match the camera server.
3. Update `run_dir` to a new, descriptive folder name for the current test 
   (e.g. `../YYYYMMDD-material-config-RV-RT/`).
4. Run all cells in order. The `dryNum` counter tracks which drying insert 
   well to use next and increments automatically — reset it to `0` when 
   loading a fresh tip rack.
5. Images are saved to `run_dir` and analyzed automatically at the end of the run.
6. Call `cam.close()` once all trials for the session are complete.

## Notes

- Each drying/droplet cycle uses fresh disposable tips (drying inserts and 
  drypad tips are single-use, discarded after each application).
- The sessile drop cycle runs 10 times per lens by default — adjust the 
  `range(10)` loop in the main workflow cell to change this.
