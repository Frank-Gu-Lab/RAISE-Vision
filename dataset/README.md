# Dataset

This folder contains the contact angle (CA) imaging dataset for the RAISE-Vision project: raw images captured during measurement runs, the processed/annotated versions of those images produced by different CA-fitting methods, and a summary spreadsheet of the resulting measurements.

## Structure

```
dataset/
├── Data Summary.xlsx
├── Raw Images/
│   ├── Raw Manual Images/
│   │   ├── Run 1/  Run 2/  Run 3/      
│   └── Raw RAISE-Vision Images/
│       ├── Run 1/  Run 2/  Run 3/      
└── Processed Images/
    ├── SCA20/
    │   └── Manual Run 1/ Run 2/ Run 3/       
    ├── Tangent Method/
    │   ├── Manual Run 1/ Run 2/ Run 3/
    │   └── RAISE-Vision Run 1/ Run 2/ Run 3/
    ├── Elliptical Fit Method/
    │   └── RAISE-Vision Run 1/ Run 2/ Run 3/
    └── Combined Method/
        ├── Manual Run 1/ Run 2/ Run 3/
        └── RAISE-Vision Run 1/ Run 2/ Run 3/
```

### Raw Images

Unprocessed source images from each measurement run:

- **Raw Manual Images** — images captured/measured manually, 3 runs.
- **Raw RAISE-Vision Images** — images captured via the RAISE-Vision system, 3 runs.

### Processed Images

The same source images after contact angle detection/annotation, organized by the fitting method used to compute the angle:

- **SCA20** — measurements from the SCA20 reference software (manual runs only).
- **Tangent Method** — tangent-line CA fitting, applied to both manual and RAISE-Vision runs.
- **Elliptical Fit Method** — elliptical-fit CA fitting, applied to RAISE-Vision runs.
- **Combined Method** — combined/ensemble fitting approach, applied to both manual and RAISE-Vision runs.

### Data Summary.xlsx

Summary spreadsheet of the measured contact angles, with one sheet per method:

- `SCA20`, `Combined Methods`, `Elliptical Fit`, `Tangent`

Each sheet reports, per cycle number and per run (Run 1–3): Left CA, Right CA, and Average CA (degrees), followed by an overall Average and Std Dev across runs.
