# Task: Implement the Next Epic (Driven by sprint-status.yaml)

## Objective
Implement all stories belonging to the current epic that needs work, as determined by the state in `sprint-status.yaml`. The epic to work on is **not hardcoded** — it must be selected dynamically based on the sprint status file each time this task runs.

## Setup — Determine Which Epic to Work On
1. Locate and read `sprint-status.yaml` in the repo root (or wherever it exists).
2. Determine the target epic using this priority order:
   - **a. In-progress epic:** If any epic has stories already marked in-progress (started but not completed/reviewed), resume that epic first.
   - **b. Next not-started epic:** Otherwise, pick the lowest-numbered epic (e.g. Epic 1, 2, 3, 4...) that has stories not yet started, in the sequence they appear in `sprint-status.yaml`.
   - **c. If all epics are complete except retrospectives:** skip to the **Retrospective** section below for the relevant epic.
3. Once the target epic `N` is identified, build a task list of all its stories (`N.x`) from `sprint-status.yaml`, respecting the order/dependencies noted there, and skipping any story already marked complete (story file created + implemented + reviewed + status synced).

## Per-Story Workflow
For **each** story `N.x` in the selected epic's task list, execute the following steps **in order**:

### Step 1 — Create Story File
- Find and open the skill file for **`bmad-create-story`** in the codebase.
- Follow that skill's steps to create the story file for `N.x` (e.g. `stories/N.x.*.md` or whatever convention this repo uses).
- This story file becomes the implementation spec for the remaining steps. Do not proceed to Step 2 until it exists and is complete per the skill's process.

### Step 2 — Implementation
- Find and open the skill file for **`bmad-agent-dev`** in the codebase.
- Follow that skill's instructions to implement the story's requirements based on the story file created in Step 1.
- Do not proceed to Step 3 until implementation is complete and consistent with the story file's acceptance criteria.

### Step 3 — Code Review
- Find and open the skill file for **`bmad-code-review`** in the codebase.
- Follow that skill's instructions to review the code implemented in Step 2.
- Resolve any issues raised by the review before marking the story as done.

### Step 4 — Sync Sprint Status
- After completing Steps 1–3 for a story, update `sprint-status.yaml` to reflect the current state of that story (status, completion, and any other fields the file's schema expects).
- Keep this update scoped to the story just completed — don't batch updates at the end.

Repeat Steps 1–4 for every remaining story in the selected epic before moving on.

## Retrospective
Once **all** stories in the selected epic have their story files created, are implemented, reviewed, and reflected in `sprint-status.yaml`:
1. Check `sprint-status.yaml` for the retrospective task associated with that epic.
2. Find and open the skill file for **`bmad-retrospective`** in the codebase.
3. Follow that skill's instructions to complete the retrospective task.
4. Update `sprint-status.yaml` to mark the retrospective as complete.

## Loop / Continuation
- After finishing an epic (including its retrospective), re-check `sprint-status.yaml` to see if another epic is ready to start.
- If the caller only wants a single epic handled per run, stop here after finishing the current one and report status. Otherwise, repeat the full process (Setup → Per-Story Workflow → Retrospective) for the next epic found.

## Constraints & Notes
- Never hardcode an epic number — always derive the target epic from the current state of `sprint-status.yaml`.
- Always locate the actual skill files in the codebase before acting — do not assume their contents or paths; find and read them fresh for each step.
- `sprint-status.yaml` is the single source of truth for progress — it must always reflect the real, current state of work after each step, not just at the end.
- Process stories strictly one at a time: create story file → implementation → review → status sync, before starting the next story.
- If a skill file or the retrospective task cannot be found, stop and report what's missing rather than guessing at its content.
