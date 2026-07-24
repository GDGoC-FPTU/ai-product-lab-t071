# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Group assignment for **Lab 02: AI Product Scoping (Vin Smart Future)** — a GitHub Classroom lab. Role-play as an **AI Engineer at Vin Smart Future**, pick a real operational pain point from a Vingroup subsidiary (VinFast, Xanh SM, Vinhomes, Vinmec, Vinpearl), scope a 6-field problem statement, and ship a programmatic Gemini prompt prototype with verified safety boundaries. The full student guide is in [README.md](README.md); the scoring rubric is in [01-worksheet.md](01-worksheet.md).

The worked example in [02-deliverable-example.md](02-deliverable-example.md) shows the target shape of an excellent submission (Xanh SM "dispatch co-pilot for stranded EVs" — SoC < 5% mobile-charger dispatch).

## Required deliverable layout (repo root)

```
.
├── 01-problem-scan.md         # Phase 1 SCAN + Phase 2 QUICK-ASSESS (×3 cards)
├── 02-deep-dive-report.md     # Phase 3 DEEP-DIVE + Phase 5 EVALUATE; group header at top with name + MSSV per member
├── 03-ai-log.md               # Phase 6 personal reflection: what AI helped with, what it got wrong, what you fixed
├── 04-workflow-diagram.png    # Hand-drawn current-state workflow (also accepts .jpg/.jpeg/.pdf); use 🔴 bottleneck + 🔄 handoff marks
└── starter-code/prompt_prototype.py   # (or extras/prompt_prototype.py) — Python prototype stress-testing Gemini boundaries
```

`.venv/`, `.env`, `__pycache__/` are git-ignored.

## Environment setup

```powershell
# Windows PowerShell
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install google-genai google-generativeai pytest
```

If activation is blocked by execution policy: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process` first.

API key (Gemini 2.5 Flash — `gemini-2.5-flash` is hardcoded as `GEMINI_MODEL` in the starter):
```powershell
$env:GEMINI_API_KEY="AIzaSy..."
python -c "import os; print('OK' if os.getenv('GEMINI_API_KEY') else 'MISSING')"
```

`requirements.txt` is intentionally empty — install from the README command above.

## Commands

### Run the prompt prototype (stress-tests boundaries against Gemini)
```bash
python starter-code/prompt_prototype.py
# from extras/ or repo root also works — autograder looks in all 3
```
Exits non-zero if `GEMINI_API_KEY` is unset. The script prints `✅ Rule N Passed` / `❌ Rule N Failed` lines that the autograder greps for.

### Run the autograder locally
The GitHub Classroom workflow calls `python autograder/autograder.py` with one of these flags per check:

```bash
python autograder/autograder.py                 # full run: sections A (files) + B (code), max 10pts
python autograder/autograder.py --section-a     # files only, max 5pts
python autograder/autograder.py --section-b     # code only, max 5pts

python autograder/autograder.py --check-file-1  # 01-problem-scan.md exists
python autograder/autograder.py --check-file-2  # 02-deep-dive-report.md exists
python autograder/autograder.py --check-file-3  # 03-ai-log.md exists
python autograder/autograder.py --check-file-4  # 04-workflow-diagram.{png,jpg,jpeg,pdf} exists
python autograder/autograder.py --check-code-1  # SYSTEM_PROMPT defined with safety keywords
python autograder/autograder.py --check-code-2  # evaluate_prompt() exists, uses genai/generativeai SDK
python autograder/autograder.py --check-code-3  # ADVERSARIAL_TESTS: ≥2 cases, dict with input+expected_violation
python autograder/autograder.py --check-code-4  # prototype script runs to exit 0 within 30s
python autograder/autograder.py --check-code-5  # output has ≥2 "Passed" and 0 "Failed" assertion lines
```

Autograder notes:
- `find_student_file()` probes `extras/prompt_prototype.py` → `starter-code/prompt_prototype.py` → `prompt_prototype.py` in that order.
- `--check-code-1` greps `SYSTEM_PROMPT` (case-insensitive) for at least 2 of: `draft_only`, `5%`, `dispatch_mobile_charger`.
- SYS_PROMPT check rejects any string still containing `TODO:` or `"Write your strict"` — replace template placeholders before submitting.

### CI
[`.github/workflows/classroom.yml`](.github/workflows/classroom.yml) runs 9 graders via `classroom-resources/autograding-command-grader@v1`, one per `--check-*` flag above, with weights: problem-scan=10, deep-dive=12, ai-log=12, workflow-diagram=14, system_prompt=10, evaluate_prompt=10, adversarial_tests=10, prototype-script=10, safety-verification=10 (sum 98). Push to any branch triggers it (excluded for the `github-classroom[bot]` actor).

## Phase workflow

| Phase | Output | Notes |
|---|---|---|
| 0 | Worked example read | [02-deliverable-example.md](02-deliverable-example.md) |
| 1 SCAN | 5+ rows in `01-problem-scan.md` table | Apply 4 Lenses: Repetitive, Time-consuming, AI-upgrade, Stakeholder Pain |
| 2 QUICK-ASSESS | 3 filled cards (same file) | Each card needs Actor, 3-5 step workflow, bottleneck with minute estimate, AI step, numeric metric, AI-fit checkbox (No AI / Rule / LLM / Agent — usually LLM Feature) |
| 3 DEEP-DIVE | `02-deep-dive-report.md` sections 1-3 | 6-field Problem Statement, Future-State Flow with 🔵 AI / 🟢 HITL / ↩️ Fallback marks, 04-workflow-diagram.png |
| 4 PROTOTYPE | `starter-code/prompt_prototype.py` | SYSTEM_PROMPT + evaluate_prompt() + ≥2 ADVERSARIAL_TESTS |
| 5 EVALUATE | Section "Evaluate" in deep-dive | AI Readiness Checklist + GO / NOT YET / NO-GO with cost estimate |
| 6 REFLECTION | `03-ai-log.md` | AI what-helped / what-was-wrong / what-you-fixed |

## Hardcoded operational boundaries

The autograder looks for these keywords in `SYSTEM_PROMPT` — keep them verbatim:

1. **`[DRAFT_ONLY]` tag** on every dispatch action (prevents automated sending without HITL).
2. **SoC < 5%**: never recommend a charging station >5 km away; instead emit `{"action": "dispatch_mobile_charger", "reason": "..."}`.
3. **HITL required**: AI is advisory only; human dispatcher must approve.

The Gemini model is fixed at `gemini-2.5-flash` and `temperature=0.0` is set in `evaluate_prompt()` (do not raise — temperature drift breaks boundary compliance, see [03-ai-log.md](03-ai-log.md) for a real-world example of why).

## Conventions

- Vietnamese in all narrative deliverables (Bối cảnh, Phân tích, Nhật ký). Keep code identifiers, log tags (`[DRAFT_ONLY]`, `dispatch_mobile_charger`, `[DRAFT_ONLY]`, `[RUNNING]`, `[Verification Checks]`), and JSON keys in English.
- Use `"..."` Vietnamese double quotes (the worksheet shows them — `"xe đi qua gờ giảm tốc kêu cụp cụp ở bánh trước"`).
- Metric style: before→after with concrete numbers (e.g. "60 phút → dưới 10 phút", not "faster"). Cost estimates should include Gemini API $/month, engineer FTEs, and a break-even ROI note.
- Always mark Human-in-the-loop steps (🟢) and Fallback paths (↩️) in the Future-State Flow — losing either drops components in G3.
