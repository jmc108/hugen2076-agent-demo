# HUGEN 2076 — Codex mini-demo

This repository contains the **synthetic six-row participant export** used in
the lecture *Working with AI Coding Agents* (slides 15–21; appendix slides 25–27).
There are no real participant records.

## Starting contents

- `raw/participants.csv` — untouched fictional export
- `docs/codebook.md` — authoritative definitions, transformations, and output requirements
- `AGENTS.md` — standing instructions for Codex
- `R/`, `validation/`, `outputs/`, `reports/` — initially empty working directories

The transformation and validation scripts **are not supplied**: Codex develops
them during the demonstration. `run_demo.R` is also intentionally absent at
the start. The codebook is the authority for data meaning and output structure.

## Start the demo

From the repository root:

```bash
codex --sandbox workspace-write --ask-for-approval on-request
```

Inside Codex, use `/status` to inspect the active session.

### Stage 1 — audit and validation plan

Paste this prompt:

> Read AGENTS.md and docs/codebook.md. Inspect raw/participants.csv.
> Do not edit implementation files yet. Write reports/audit.md with row count,
> candidate key, missing-code counts, and values outside documented ranges.
> Delegate one validation subagent to draft reports/validation_plan.md from
> the codebook and the hand-checked facts. Wait for it, summarize the findings,
> and stop.

**Review checkpoint:** Expect six records; a whitespace-padded ID `001`;
`-9` once in age and once in weight; `sex_code=9` means unknown;
and an out-of-range height of `450` for subject `006`.
These are checks on the source, not permission to change it.

### Stage 2 — implement and verify

After approving the documented transformations, paste:

> Implement only the approved rules. Use base R.
> Main agent: create R/clean_participants.R and run_demo.R;
> write outputs/participants_clean.tsv, reports/issues.tsv, and
> reports/summary.md.
> Validation subagent: create validation/check_outputs.R.
> Do not edit implementation files. Draft checks from the codebook and
> hand-checked facts; run them only after outputs exist.
> Run Rscript --vanilla run_demo.R and
> Rscript --vanilla validation/check_outputs.R.
> Report commands run, changed files, and remaining failures. Stop.

Use `/subagents` to inspect the validator's actual work and `/diff` to
inspect changes. From a separate terminal, rerun:

```bash
Rscript --vanilla run_demo.R
Rscript --vanilla validation/check_outputs.R
```

### Acceptance criteria

- Six rows, same order, IDs preserved as text (`001`, not `1`).
- Documented missing codes become missing values.
- `sex_code=9` becomes `unknown`, not an error.
- Subject `006` has a missing cleaned height, and the original `450`
  is recorded in `reports/issues.tsv`.
- No other values change unexpectedly.
- The exported files pass independent validation.
- The pipeline runs without asking Codex to reconstruct it.

## Notes

`AGENTS.md` is supplied already, matching the lecture's mini-demo appendix.
Do **not** run `/init` in this exercise unless you intentionally want to
replace or revise those instructions. It is not a security boundary:
`workspace-write` normally allows edits within this project, including
`raw/`, despite the instruction not to change it.

This is the *small live example*, not the larger three-table project previewed
for the following week.
