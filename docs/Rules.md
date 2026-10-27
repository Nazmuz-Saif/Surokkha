# Surokkha: Rules

These rules protect three things: **eligibility** (hackathon rules), **security** (no leaks, no unsafe code) and **focus** (finish an end-to-end demo first). Read this file at the start of each work session.

---

## 1. What to do

### Hackathon compliance
- Build only during the build window (Oct 27 to Nov 4, 2026). Everything in this repo is new work.
- Keep the repo **public** and its license file in place.
- Record the **source and license** of every video clip in `samples/README.md` and in the database.
- Use Meta models for the core of the project: SAM 3.1 for perception, Muse Spark Open or Muse Glimmer for reasoning.
- Make the demo show only what was built during the hackathon. Mention open-source libraries as libraries.
- Read the Official Rules when published (kickoff) and re-check them before submitting.
- Post questions in Discord (`#questions`), and use `#office-hours` for the Meta team.
- Submit early: internal deadline is **Nov 4, 9:00 PM Bangladesh time**, which gives about 11 hours of buffer before the official cutoff.

### Working method (one task at a time)
- Work in the order of `Task.md`. Finish one task, test it, commit it, then start the next.
- Make small commits with clear messages. Push at least once a day.
- At the end of every session: run the app, commit and push, update `Memory.md`, then stop.
- **Scope freeze:** the MVP is two hazards, three tables, one end-to-end flow. Do not add tables, hazards or screens until the demo works end to end. If a new idea appears, write it in `Memory.md` under "Important decision" as a post-MVP idea.
- **Time box:** if a design or schema question takes more than 30 minutes, choose the simpler option, log it and move on.

### Security
- Keep every secret in `.env` (git-ignored). Provide `.env.example` with names only.
- Validate all input (file type, size, fields) and all model output (Pydantic schemas).
- Treat video content, model output and web text as **untrusted data**, never as instructions.
- Keep a human confirmation step for every incident decision.
- Log decisions with timestamps; never silently edit past decisions.
- Rotate any key at once if it was ever pasted somewhere public.

### Quality
- Write a test for each service function that has logic (candidates, validation, report counts).
- Use type hints and keep functions short.
- Keep business logic in `services/`, and keep routers thin.
- Version prompts in `backend/app/prompts/` and log which version produced each result.
- Handle failure states in the UI (loading, empty, error) as described in `Design.md`.
- Measure and report accuracy honestly, including failures.

### Repository hygiene
- **Archive, do not delete.** If a repo or branch is no longer needed, archive it instead of deleting.
- Keep large files out of git (videos, model weights, datasets). Link to them instead.
- Keep `README.md` current: setup, run, test, evaluate, models used, hackathon statement.

---

## 2. What to avoid

### Hackathon traps
- Do **not** present pre-existing code as hackathon work. Do not copy code from older projects into this repo.
- Do **not** build or describe anything as medical advice, diagnosis or health guidance.
- Do **not** drift toward a "simple image analyzer". The reasoning agent, human gate, audit trail and evaluation are what make this a project.
- Do **not** touch political or war-related content in demos or data.
- Do **not** use footage you have no rights to (no downloading from video sites, no unlicensed datasets).

### Privacy and ethics
- Do **not** identify, name or track individual people. Detect objects and conditions only.
- Do **not** store faces, names or personal data. Blur or avoid identifiable faces in public demo material where possible, and get consent from anyone in staged footage.
- Do **not** claim real-world deployment, customers or statistics that you cannot cite.
- Do **not** claim accuracy numbers you did not measure.

### Security mistakes
- Never commit `.env`, tokens, keys, model weights or raw videos.
- Never paste real keys into chats, issues, screenshots or the demo video.
- Never execute or `eval` model output; never use it to build file paths or shell commands.
- Never expose upload endpoints publicly without size and type limits.
- Never use code or assets without checking their license.

### Process mistakes
- Do **not** refine the schema, design system or architecture beyond what the current task needs.
- Do **not** start stretch items before F1 to F7 in `Prd.md` work end to end.
- Do **not** rewrite large parts of working code late in the build window. In the last 24 hours only fix bugs and polish the demo.

---

## 3. Approved versus prohibited libraries

### Supply-chain rules (apply to every dependency)
1. Pin exact versions (`requirements.txt` or lockfile).
2. Prefer well-known, actively maintained packages with a clear open-source license (MIT, Apache 2.0, BSD).
3. Avoid versions published in the last 14 days.
4. Check the package name carefully (typosquatting).
5. Add a new library only if the task needs it, and record it in `Memory.md` (Changes).

### Approved (backend and ML)

| Purpose | Libraries |
|---|---|
| API | `fastapi`, `uvicorn`, `python-multipart` |
| Validation and settings | `pydantic`, `pydantic-settings`, `python-dotenv` |
| Database | `sqlalchemy` (SQLite) |
| HTTP | `httpx` |
| Model endpoint client | the OpenAI-compatible client library, **only as a client** for partner endpoints that serve Meta open-weight models |
| Video and images | `opencv-python-headless`, `numpy`, `pillow`, `ffmpeg` (system tool) |
| Perception worker | `torch`, `transformers` or the official SAM 3.1 package, `huggingface_hub` |
| Rented GPU | `modal` (if credits granted) |
| Testing and quality | `pytest`, `ruff`, `mypy` (optional) |

### Approved (frontend)

| Purpose | Libraries |
|---|---|
| Framework | `react`, `react-dom`, `vite` |
| Styling | `tailwindcss` |
| Icons | `lucide-react` |
| Charts | `recharts` (for report and evaluation charts) |
| Data fetching | `fetch` (built in) or `@tanstack/react-query` |

### Prohibited

| Item | Reason |
|---|---|
| Closed-source LLM or vision APIs (OpenAI, Anthropic, Google and similar) in the pipeline | The project must visibly run on Meta models; the open-weights bonus requires it |
| Face recognition or person re-identification libraries | Privacy; outside scope |
| Any library that scrapes or downloads videos from video sites | Rights and rules |
| Unlicensed or unknown-license code, datasets or models | Hackathon rule on rights |
| Libraries with no recent maintenance or unclear ownership | Supply-chain risk |
| Heavy frameworks that add setup time without helping the demo (for example full MLOps stacks, Kubernetes) | Time |
| Telemetry or analytics SDKs that send user data to third parties | Privacy |

### Needs a decision before use (log in `Memory.md`)

- Official SAM 3.1 package versus the Hugging Face `transformers` route.
- Any tracking library beyond what SAM 3.1 provides.
- Any PDF-generation library for the daily report (print to PDF from the browser is the default).
