# Surokkha: Memory

The project log. Update it at the end of every work session and whenever a decision, change or bug appears. This file is **public** (it lives in the repo), so never write secrets, keys, personal phone numbers or private emails here.

---

## 1. Important decision

Format: `Date | Decision | Why | Alternatives considered`

| Date | Decision | Why | Alternatives considered |
|---|---|---|---|
| 2026-10-27 | Drop the earlier prescription-reading idea | The hackathon guide lists AI medical advice and simple image analyzers as anti-projects | Keep it with a narrower scope (rejected: too risky) |
| 2026-10-27 | Project is **Surokkha**, a garment-factory safety agent | Fits the "Push the Frontier: manufacturing" theme; real-world problem; two Meta models can work together | Voice agent for government forms (needs Muse Wonder and a card; not available), WhatsApp shop agent (crowded market) |
| 2026-10-27 | Build on **open-weight models** (SAM 3.1 plus Muse Spark Open, Glimmer as fallback) through partner inference | Organizers confirmed the Meta Model API needs a payment method and that open weights via partners are the path | Meta Model API with Wonder or Voice Transcribe (blocked without a card) |
| 2026-10-27 | **Indie Developer** track, solo | Solo student builder; track judged on creativity, ambition and whether it works | Startup track (not a fit for a new project) |
| 2026-10-27 | Public repo `Nazmuz-Saif/Surokkha` with a license; all code is new | Hackathon rules: public repo and new work only | None |
| 2026-10-27 | MVP scope locked: 2 hazards (missing PPE, blocked exit), 3 tables, one end-to-end flow | Avoid analysis paralysis; finish the demo first | Wider hazard list (post-MVP) |
| 2026-10-27 | Proposed stack: FastAPI, SQLite, React with Vite and Tailwind, SAM 3.1 worker on a rented GPU | Fast to build and easy to validate model output | Django with DRF (also fine; switch only if it is clearly faster for you) |
| *TBD (Day 2)* | Concept approach (detect `person` and `face mask` separately, then compute overlap) | To be validated on a real frame | Direct prompt "person without mask" |
| *TBD (Day 3)* | Agent method: tool calling versus structured JSON | Depends on endpoint support | |
| 2026-10-27 | Use the official SAM 3.1 GitHub repo (facebookresearch/sam3) with the HF checkpoints; no Transformers route | The HF page says it hosts checkpoints only | Hugging Face Transformers (not supported) |

**Post-MVP ideas (parking lot, do not start early):** multi-camera view, more hazard types, Bangla interface, real-time streaming.

---

## 2. Changes

Format: `Date | Change | Files or area`

| Date | Change | Files or area |
|---|---|---|
| 2026-10-27 | Created public repo, connected local folder, first commit | repo |
| 2026-10-27 | Added `docs/scope.md` | `docs/` |
| 2026-10-27 | Added `docs/Prd.md`, `Architecture.md`, `Rules.md`, `Design.md`, `Task.md`, `Memory.md` | `docs/` |

**Dependencies added** (record each one: name, version, reason, date):

| Date | Package | Version | Reason |
|---|---|---|---|
| | | | |

**Partner credit status** (update when emails arrive):

| Partner | Applied | Status |
|---|---|---|
| Baseten ($100, higher rate limit) | 2026-10-27 | Waiting |
| Modal ($100) | 2026-10-27 | Waiting |
| Fireworks ($25) | 2026-10-27 | Waiting |
| SAM 3.1 access on Hugging Face | | Not requested yet |

---

## 3. Bug

Format: `ID | Date | What happened | Cause | Fix | Status`

| ID | Date | What happened | Cause | Fix | Status |
|---|---|---|---|---|---|
| B-001 | 2026-10-27 | `git push` rejected after first commit | GitHub repo already had README or license, so remote had commits the local repo lacked | `git pull origin main --allow-unrelated-histories`, then push | Fixed |

**Bug entry template** (copy for each new bug):

```
ID: B-00X
Date:
What happened (exact error text, no secrets):
Steps to reproduce:
Cause:
Fix:
Test added: yes / no
Status: open / fixed
```

**Known limitations** (add honestly as they appear; they go into the demo):

- *(none yet)*

---

## 4. Contact

Public contacts only. No personal emails or phone numbers in this file.

| Who | Where | Use for |
|---|---|---|
| Hackathon organizers (Cerebral Valley) | Hackathon Discord, `#questions` (mention `@Cerebral Valley`) | General and submission questions |
| Meta team | Hackathon Discord, `#office-hours` (weekdays, 10:00 to 11:00 AM ET; text answers only) | Model and API questions |
| Baseten | Baseten channel in the hackathon Discord | Endpoint access, model ID `meta-models/muse-spark-open-1026` |
| Modal | Modal documentation and support | GPU and endpoint setup |
| Fireworks | Fireworks workshop (Oct 30) and the credit email | Serverless inference |
| Meta Model API support | `https://dev.meta.ai/support` | Account and API issues |
| Project repo | `https://github.com/Nazmuz-Saif/Surokkha` | Code, issues |
| Submission platform | Cerebral Valley event page (`cerebralvalley.ai/e/meta-ai-developer-hackathon`) | Final submission |

**Key dates:** build window Oct 27 to Nov 4; submission due Nov 4, 9:00 PM ET (Nov 5, about 8:00 AM Bangladesh time); judging Nov 5 to Nov 8; closing ceremony Nov 9; community vote winner Nov 16.
