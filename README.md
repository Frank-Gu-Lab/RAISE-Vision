# RAISE-Vision
**Robotic Autonomous Imaging Surface Evaluator for Vision Applications**
An automated platform for contact angle (CA) measurement on curved hydrogel surfaces, developed for standardized *in vitro* contact lens wettability analysis throughout air exposure.

## Overview

Existing sessile drop contact angle measurements for contact lenses have poor repeatability due to differences between laboratories and operators, including drying method, time to measurement, droplet volume, etc. RAISE-Vision addresses this by combining custom 3D-printed hardware with an automated liquid-handling workflow and a custom image-processing pipeline, helping operators perform both sample preparation and CA analysis more reliably and consistently.

This repository contains the hardware designs, control software, and image analysis scripts used to build and run the RAISE-Vision platform, along with the benchmarking data and statistical analysis reported in the associated manuscript.

RAISE-Vision builds on the RAISE platform (Nazeri et al., *Digital Discovery*, 2026) for automated formulation-linked wettability measurement, tailored here for curved contact lens substrates.

## Repository Structure

| Folder | Description |
|---|---|
| [`hardware/`](hardware) | 3D-printable STL files for the drypad assembly, lens mount, and base stage |
| [`labware/`](labware) | Physical labware used alongside the custom hardware and JSON definitions |
| [`JSON Files/`](JSON%20Files) | Custom Opentrons labware definitions (stage, drypad tip rack, trash) matching the hardware above |
| [`protocol scripts/`](protocol%20scripts) | OT-2 control notebook and host-PC code (camera control, client/server link, image analysis) that run a full automated measurement cycle |
| [`Image Processing/`](Image%20Processing) | Standalone contact angle image-processing notebook — same analysis pipeline as `protocol scripts/`, but for processing images independently of the OT-2/camera setup |
| [`dataset/`](dataset) | Raw and processed images from benchmarking runs, plus a summary spreadsheet of measured contact angles across fitting methods |

## Getting Started

1. **Print the hardware** — see [`hardware/`](hardware) for STL files (drypad, lens mount, stage).
2. **Load the custom labware** — import the definitions in [`JSON Files/`](JSON%20Files) into the Opentrons App before running any protocol.
3. **Run an automated measurement** — see [`protocol scripts/`](protocol%20scripts) for the OT-2 notebook, camera server, and environment setup.
4. **Process images independently** (optional) — if you already have CA images and want to analyze them without the OT-2 setup, see [`Image Processing/`](Image%20Processing).
5. **Reference data** — see [`dataset/`](dataset) for the raw/processed benchmarking images and measurement summary used in the manuscript.

## Contact

For questions about this repository, please open an issue or contact Frank Gu (f.gu@utoronto.ca).
