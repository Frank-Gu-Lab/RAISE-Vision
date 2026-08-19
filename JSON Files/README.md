# JSON Files

Custom Opentrons labware definitions for the RAISE-Vision platform. Each JSON file defines the dimensions, well positions, and offsets for a custom 3D-printed part so the Opentrons pipetting robot can accurately interact with it.

| File | Description |
|---|---|
| `raise_vision_drypad_tiprack.json` | Labware definition for the drypad tip rack |
| `raise_vision_stage.json` | Labware definition for the RAISE-Vision stage |
| `raise_vision_trash.json` | Labware definition for the trash for drying apparatus |

## Usage

Load these definitions into the Opentrons App (or via the Opentrons Python API) as custom labware before running protocols that reference the RAISE-Vision hardware.

## Design source

Corresponding physical parts and STL files for these labware definitions are in the [`hardware/`](../hardware) folder.
