# Member 3 — Density Estimation & Congestion Alerts

## Overview
This module computes traffic density from vehicle counts, classifies it into
bands (LOW / MED / HIGH), triggers congestion alerts, logs data to CSV, and
renders annotated overlays on video frames using OpenCV.

---

## Files

| File | Purpose |
|------|---------|
| `density.py` | Core algorithm: density score, band classification, alert logic, CSV logging |
| `alerts.py` | OpenCV display: density bar, band label, red alert banner |
| `config.py` | Configuration parameters (thresholds, CSV path) |
| `test_density.py` | Unit tests (pytest) |

---

## Algorithms

### Density Score
```
density = min(vehicle_count / HIGH_THRESHOLD, 1.0)
```
Produces a value in **[0.0, 1.0]**, capped at 1.0 for extreme counts.

### Band Classification
| Condition | Band |
|-----------|------|
| count ≤ 5 | LOW |
| count ≤ 10 | MED |
| count > 10 | HIGH |

### Alert Trigger
An alert fires when `vehicle_count > THRESHOLDS["high"]` (default 20).

### CSV Log
Each frame with a logged event appends one row:
```
timestamp, frame, vehicle_count, band
```

---

## Configuration (`config.py`)

```python
THRESHOLDS = {
    "low":  5,
    "med":  10,
    "high": 20,
}
CSV_LOG_PATH = "density_log.csv"
```

---

## OpenCV Display Modules (`alerts.py`)

| Function | What it draws |
|----------|--------------|
| `draw_band_label(frame, count)` | Colour-coded `DENSITY: LOW/MED/HIGH` text (top-left) |
| `draw_density_bar(frame, count, density)` | Vertical fill bar on the right edge |
| `draw_alert_banner(frame, count)` | Red semi-transparent banner (top) when alert is active |
| `annotate_frame(frame, count, density)` | Convenience wrapper — calls all three above |

---

## Integration with `main.py`

```python
from density import compute_density, classify_density, log_count_to_csv
from alerts import annotate_frame

# Inside the frame loop (after tracker gives vehicle_count):
density = compute_density(vehicle_count)
frame   = annotate_frame(frame, vehicle_count, density)
log_count_to_csv(frame_number, vehicle_count, classify_density(vehicle_count))
```

---

## Running Tests

```bash
pip install pytest opencv-python numpy
python -m pytest test_density.py -v
```

All 20 tests should pass with no display window required.

---

## Dependencies
- `opencv-python`
- `numpy`
- `pytest` (tests only)
- Standard library: `csv`, `os`, `datetime`
