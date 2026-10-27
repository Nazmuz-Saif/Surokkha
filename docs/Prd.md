# Surokkha: Product Requirements Document (PRD)

| | |
|---|---|
| **Project** | Surokkha ("safety" in Bangla) |
| **Event** | Meta Global AI Developer Hackathon (build window Oct 27 to Nov 4, 2026) |
| **Track** | Indie Developer (solo) |
| **Primary theme** | Push the Frontier: manufacturing |
| **Submission due** | Nov 4, 2026, 9:00 PM ET (Nov 5, ~8:00 AM Bangladesh time). Internal deadline: Nov 4, 9:00 PM Bangladesh time |
| **Status** | MVP scope locked on Oct 27. Changes go through `Memory.md` (Important decision) |

---

## 1. Execution summary

**What it is.** Surokkha is an agentic safety monitor for garment factories. It takes factory video, finds safety hazards, builds an incident timeline with visual evidence, drafts a supervisor alert, and generates a daily safety report. A human confirms or rejects every alert before anything is "sent".

**How it works (one paragraph).** Video is split into frames. SAM 3.1 finds and tracks concepts in the frames from short text prompts (for example `person`, `face mask`, `door`, `box`). A Meta open-weight model (Muse Spark Open, with Muse Glimmer as fallback) reasons over those detections and the key frames: it decides which detections are real hazards, merges them into incidents, writes the timeline and drafts the alert. A React dashboard lets a safety officer review evidence and confirm or reject. Accuracy is measured on 20 labeled clips and reported honestly.

**MVP hazards (only two):**
1. **Missing PPE:** a worker without the required PPE (default: face mask; head cover or helmet is configurable).
2. **Blocked exit or aisle:** an obstruction (boxes, carts) in or near an exit or marked aisle.

**What judges will see:** a public GitHub repo, a 3-minute demo video, a live demo, and a measured accuracy table.

### Hackathon fit

| Judging criterion | Weight | How Surokkha scores |
|---|---|---|
| Impact | 30% | Garment safety is a high-stakes, Bangladesh-relevant problem; a safety officer is a clear user. |
| Use of Meta AI tooling | 25% | SAM 3.1 for concept detection and tracking plus Muse Spark Open or Glimmer as the reasoning agent. Two Meta models working together, not a basic chat integration. |
| Technical depth and execution | 20% | Detection, geometric reasoning, agent loop with validated structured output, human-in-the-loop gate, audit trail, measured precision and recall. |
| Demo | 25% | A clear before-and-after story in 3 minutes: video in, hazard found, alert drafted, human confirms, report out. |

**Bonus prizes we are targeting (none guaranteed):**
- **Best Build on Meta Open Weight Models ($10K):** Spark Open or Glimmer served through partner inference. Keep the agent on open weights and keep proof (endpoint logs, model IDs) for the partner check.
- **Best Agentic Use-Case ($5K):** multi-step autonomy (detect, reason, draft, report) with a human gate.
- **Community Vote ($10K) and Most Viral ($20K):** a short, shareable demo clip. Posted on X with the submission link.

### Explicit non-goals

Real-time CCTV streaming, multi-factory support, user accounts and login, mobile app, medical or personal-advice features, and anything political. Surokkha does not identify or track individuals by identity; it detects objects and conditions only.

---

## 2. Problem statement and target audience

### Problem

Bangladesh's garment industry is the country's largest export sector, and it has a serious history of factory safety failures (for example the 2013 Rana Plaza collapse and several factory fires), which led to international safety inspection programs. Day-to-day safety monitoring still depends heavily on people: inspectors walking the floor and supervisors who cannot watch every line, every exit, every shift.

Hypothesis behind Surokkha: cameras already exist in many factories, but nobody has time to watch them. An agent that watches for a small set of high-value hazards and hands a human a short, evidence-backed list could shorten the time between a hazard appearing and someone fixing it.

> **TODO before the demo (do not skip):** collect 2 to 3 citable sources for the statements above and put the links in `docs/sources.md`. Do not invent statistics. Treat the "hypothesis" paragraph as a hypothesis and say so in the demo.

### Target audience

| Persona | Role | Job to be done | What they need from Surokkha |
|---|---|---|---|
| **Primary: Safety / compliance officer** | Owns safety checks and audit readiness | Find hazards early, prove they were handled | Evidence clips, a confirm/reject queue, a daily report |
| **Secondary: Floor supervisor** | Runs a line or a floor | Fix issues fast | A short alert: what, where, when, with one image |
| **Tertiary: Auditor or buyer representative** | Reviews compliance | See a trustworthy record | An audit trail of detections, decisions and who confirmed |

### Why now

Open-weight models are now small enough to run on rented GPUs, SAM 3.1 can find objects from short text prompts, and Muse Spark Open can reason over what was found. The combination makes a monitor like this buildable by one developer in days.

---

## 3. Core features

Each feature has acceptance criteria. A feature is done only when all criteria pass.

### F1. Video ingest
- Upload a short video (MP4) through the dashboard. Store metadata including the **footage source and license** (required field).
- Extract frames at a configurable rate (default 1 frame per second, with a hard cap on frames per video to control cost).
- *Done when:* upload, frame extraction and a stored video record work end to end on 3 sample clips.

### F2. Concept detection with SAM 3.1
- Run text-prompted detection and tracking for a small concept set (`person`, `face mask`, `helmet`, `door`, `box`, `cart`).
- Save detections as structured records (frame, time, concept, confidence, box or mask reference, track id).
- *Done when:* detections for all concepts are saved for a clip and can be drawn over the frame.

### F3. Hazard reasoning agent (Muse Spark Open)
- Combine detections with simple geometry (for example a `person` with no overlapping `face mask`) and let the model confirm each candidate using the key frame and the detection JSON.
- The model must return **structured JSON** that is validated; invalid output is retried, then rejected and logged.
- *Done when:* the agent produces incident records with type, time range, severity, a short explanation and evidence references.

### F4. Incident timeline and evidence
- Dashboard timeline of incidents with the key frame, detection overlays and the agent's explanation.
- *Done when:* every incident shows its evidence and links back to the exact detections that support it.

### F5. Human confirmation gate and alert drafts
- For each incident the agent drafts a short supervisor alert (English, with optional Bangla text as a stretch).
- A human **confirms or rejects** each incident. Nothing is marked "sent" without confirmation. All decisions are logged with a timestamp.
- *Done when:* confirm and reject both work and the status is stored.

### F6. Daily safety report
- Generate a summary: counts by hazard type, confirmed versus rejected, and the top incidents with evidence.
- *Done when:* a report renders for a processed video set and can be exported (print to PDF is enough).

### F7. Evaluation page
- Run the pipeline on 20 labeled clips and show precision and recall per hazard. Show the failures too.
- *Done when:* the table is generated by a script that anyone can re-run from the repo.

### Stretch (only after F1 to F7 work end to end)
- **S1:** alert delivery through the WhatsApp Business Cloud API using a test number.
- **S2:** Bangla alert text.
- **S3:** model routing proof for the open-weights bonus (logs showing which model handled which step).

---

## 4. Success metrics

| Metric | Target | How measured |
|---|---|---|
| End-to-end demo works | Passes 5 consecutive dry runs | Manual checklist in `Task.md` |
| Missing-PPE detection | Report precision and recall (aim for a clear, honest number) | Eval script on 20 labeled clips |
| Blocked-exit detection | Report precision and recall | Same script |
| Time from upload to incident list | Short enough for a live demo (target under 2 minutes for a 30-second clip) | Timer in the pipeline logs |
| Agent output validity | Nearly all responses pass schema validation | Validation log |

Numbers are reported as measured. If results are weak, the demo says so and shows what the human gate catches.

---

## 5. Demo plan (3 minutes)

1. **0:00 to 0:20:** the problem in one sentence and one image.
2. **0:20 to 0:50:** upload a clip; show frames and SAM 3.1 detections appearing.
3. **0:50 to 1:40:** the agent flags two hazards; open an incident and show the evidence and explanation.
4. **1:40 to 2:15:** the human confirms one and rejects one (show the false alarm being caught); alert draft shown.
5. **2:15 to 2:40:** daily report and the accuracy table (with honest failures).
6. **2:40 to 3:00:** what is built during the hackathon, which Meta models were used and where, and what comes next.

The demo shows only what was built during the hackathon.

---

## 6. Risks and mitigations

| Risk | Mitigation |
|---|---|
| SAM 3.1 does not handle "person without mask" directly | Detect `person` and `face mask` separately and compute overlap; validate on Day 2 before building more. |
| No GPU for SAM 3.1 locally | Run on Modal GPU credits (if granted) or another free GPU environment. Check credits status daily. |
| Partner credits are not granted | Keep the pipeline provider-agnostic; ask organizers on Discord for options; reduce frames and cache results. |
| Footage: licensing and privacy | Only own, staged or rights-cleared footage; license recorded per video; no scraping from video sites. |
| "Simple image analyzer" label | Add the reasoning agent, the human gate, the audit trail and the evaluation. |
| Time (8 days, solo) | Scope locked; two hazards only; three tables only; stretch items start after the end-to-end demo works. |
| Model output is wrong or unsafe | Validated JSON, human confirmation, confidence shown, nothing sent automatically. |

## 7. Open questions

- Exact SAM 3.1 access and runtime requirements (gated model; check the official model card).
- Which partner credits are granted (Baseten, Modal, Fireworks).
- Muse Spark Open tool-calling support through Baseten (fallback: structured JSON output).
- Final Official Rules text (published at kickoff); re-check rules on new work, repo and demo.
