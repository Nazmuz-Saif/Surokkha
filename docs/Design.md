# Surokkha: Design

Goal: a calm, trustworthy safety tool that a safety officer can use in seconds. The design must make **evidence** and **human decisions** obvious. Direction: modern minimalist, data first, no decoration that does not help a decision.

Scope: four screens (Upload, Incidents, Report, Evaluation). Anything else waits until after the end-to-end demo works.

---

## 1. Design principles

1. **Evidence first.** Every incident shows its frame, overlays and explanation before any action button.
2. **Human decides.** Confirm and reject are always visible and clearly different. Nothing is "sent" without confirmation.
3. **Honest about uncertainty.** Show confidence and "needs human" states; never hide failures.
4. **Fast to scan.** Severity, type and time are readable at a glance.
5. **Plain language.** Short sentences; no jargon in the interface.

---

## 2. Design tokens

### Colors

| Token | Light | Dark | Use |
|---|---|---|---|
| `--bg` | `#F7F7FB` | `#0E0E1A` | Page background |
| `--surface` | `#FFFFFF` | `#14142B` | Cards, panels |
| `--text` | `#14142B` | `#ECECF5` | Main text |
| `--text-muted` | `#5B5B78` | `#A4A4C0` | Secondary text |
| `--primary` | `#6C31D6` | `#8B5CF0` | Primary actions, links |
| `--border` | `#E2E2EE` | `#2A2A48` | Dividers |
| `--sev-high` | `#C62828` | `#EF5350` | High severity |
| `--sev-medium` | `#B26A00` | `#FFA726` | Medium severity |
| `--sev-low` | `#2E7D32` | `#66BB6A` | Low severity and confirmed |
| `--info` | `#1565C0` | `#64B5F6` | Neutral information |

Rules: do not rely on color alone (always add an icon or text label for severity and status). Keep text contrast at WCAG AA or better.

### Typography

- Headings: **Inter** (600 to 700). Body: **Inter** (400 to 500). Monospace for ids and JSON: system monospace.
- Scale: 12, 14, 16, 20, 24, 32 px. Body text 14 to 16 px.
- Bangla text (stretch): use a Bangla-capable fallback font (for example Noto Sans Bengali).

### Spacing, radius, motion

- Spacing scale: 4, 8, 12, 16, 24, 32, 48 px.
- Radius: 8 px (controls), 12 px (cards).
- Motion: 150 to 250 ms, ease-out, only for state changes. Respect "reduce motion".

---

## 3. Layout

- Top bar: product name, nav (Upload, Incidents, Report, Evaluation).
- Content width: max 1200 px, centered; two-column layout on desktop for Incidents (list left, detail right); single column on small screens.
- Primary demo screen size: laptop (1366 px and wider). Mobile is a "should not break" target, not a design goal.

---

## 4. Screens

### 4.1 Upload

**Purpose:** add a video and start analysis.

- File drop area (MP4 only, size limit shown).
- Required fields: **source description** and **license**. The submit button stays disabled until both are filled.
- Start analysis button; progress steps: *Uploading → Extracting frames → Detecting → Reasoning → Done*.

States: empty, file selected, uploading (progress), processing (step list), done (link to incidents), error (reason and retry), invalid file (type or size message).

### 4.2 Incidents (main screen)

**Purpose:** review hazards and decide.

- **Left list:** incident cards sorted by severity then time. Each card: type icon, severity badge, time range, status chip (pending, confirmed, rejected, needs human).
- **Right detail:**
  1. Key frame with detection overlays (toggle overlays on or off).
  2. Short clip or frame strip around the incident.
  3. Agent explanation (2 to 3 sentences) and confidence.
  4. Evidence list (detection ids with concept and confidence).
  5. Alert draft (editable text).
  6. Actions: **Confirm** (primary) and **Reject** (secondary), plus an optional note.
- After a decision: status updates, timestamp shown, actions replaced by "Decision recorded".

States: empty (no incidents found: "No hazards found in this video"), loading skeleton, pending, confirmed, rejected, needs human (shown with a clear notice that the model output was invalid or uncertain), error loading.

### 4.3 Daily report

**Purpose:** summary for the safety officer.

- Date picker; summary tiles (total incidents, confirmed, rejected, needs human).
- Breakdown by hazard type (simple bar chart).
- Top incidents list with thumbnail and decision.
- Print-friendly layout (print to PDF).

States: empty (no data for the date), loading, ready, error.

### 4.4 Evaluation

**Purpose:** show honest accuracy.

- Table: hazard type, precision, recall, number of clips.
- List of failures with the frame, what the model said and what the label says.
- Button to re-run (disabled while running); timestamp of the last run.

States: not run yet, running, ready, error.

---

## 5. Components

| Component | Notes |
|---|---|
| `SeverityBadge` | Color plus icon plus text (High, Medium, Low) |
| `StatusChip` | pending, confirmed, rejected, needs human |
| `IncidentCard` | Compact, keyboard focusable |
| `FrameViewer` | Image with overlay canvas; overlay toggle |
| `EvidenceList` | Concept, confidence, link to frame |
| `DecisionBar` | Confirm, Reject, note field; disabled after decision |
| `StepProgress` | Pipeline steps with current step highlighted |
| `EmptyState` / `ErrorState` | Short message, one clear next action |

---

## 6. Accessibility and quality checklist

- [ ] All interactive elements reachable by keyboard, visible focus ring
- [ ] Color is never the only signal (icons and labels present)
- [ ] Text contrast meets WCAG AA in light and dark
- [ ] Images and overlays have text alternatives (alt text, or an accessible description of the incident)
- [ ] Buttons have clear labels ("Confirm incident", not "OK")
- [ ] Every screen has loading, empty and error states
- [ ] Destructive or irreversible actions show what will happen (a decision cannot be silently undone)
- [ ] No text baked into images; layout survives long text
- [ ] Reduced-motion preference respected
- [ ] Layout holds from 1366 px to small laptop widths; does not break on phones

## 7. Demo polish checklist (last two days only)

- [ ] Clean sample data (no real faces without consent, no test junk)
- [ ] Dashboard reads well at recording resolution (1080p)
- [ ] Large enough text for video compression
- [ ] No keys, tokens or personal data visible on screen
- [ ] One consistent hazard example used across screens and the video
