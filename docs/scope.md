# Surokkha: MVP scope (locked on Oct 27)

## Demo story (3 minutes)
A short factory video goes in. Surokkha finds hazards, builds an incident timeline,
drafts a supervisor alert, a human confirms it, and a daily report is generated.

## Hazards (only these 2 for the MVP)
1. Worker without a mask or helmet
2. Blocked exit or aisle

## Pipeline
video -> frames -> SAM 3.1 (concept prompts) -> detections (JSON)
-> Muse Spark Open agent: reasons over detections, writes timeline + alert draft
-> dashboard: human confirms/rejects -> daily report

## Data (3 tables only: videos, detections, incidents)
Do not add more tables before the end-to-end demo works.

## Out of scope
Real-time CCTV, multi-factory support, user accounts, mobile app.

## Success metric
Accuracy on 20 labeled clips (precision/recall per hazard), reported honestly in the demo.
