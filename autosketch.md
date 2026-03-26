# autosketch

This is an experiment to have the agent do its own development.

## Setup

To set up a new experiment, work with the user to:

1. **Agree on a run tag**: propose a tag based on today's date (e.g. `mar5`). The branch `sketchy/<tag>` must not already exist — this is a fresh run.
2. **Create the branch**: `git checkout -b sketchy/<tag>` from current master.
3. **Read the in-scope files**: The repo is small. Read these files for full context:
   - `README.md` — repository context.
   - `sketch.md` — the file you modify. it's a skill for Claude Code to use. it has detailed instructions for the task.
4. **Initialize results.tsv**: Create `results.tsv` with just the header row. The baseline will be recorded after the first iteration.
6. **Confirm and go**: Confirm setup looks good.

Once you get confirmation, kick off the iteration.

## Iteration

Take sketch.md and install in Claude Code as a skill. Overwrite if one already exists by the same name. The skill is used by Claude Code. You invoke it by calling `/sketch concept_sketch.png`.

**What you CAN do:**
- Modify `sketch.md` — this is the only file you edit. Everything is fair game: general instructions, specific prompts, methodology, etc.
- Install any python packages or dependencies required by Claude Code to draw using `uv add <package-name>` followed by `uv sync`.

**What you CANNOT do:**
- Modify `prepare.py`. It is read-only. It contains the fixed evaluation, data loading, tokenizer, and training constants (time budget, sequence length, etc).

**The goal is simple: get the highest score.** Everything is fair game: change the sketch.md, the tools needed. The only constraint is that skill runs without crashing.

**Simplicity criterion**: All else being equal, simpler is better. A small improvement that adds ugly complexity is not worth it. Conversely, removing something and getting equal or better results is a great outcome — that's a simplification win. When evaluating whether to keep a change, weigh the complexity cost against the improvement magnitude.

**The first iteration**: Your very first iteration should always be to establish the baseline, so you will run the skill as is.

## Output format

Once the script finishes it prints a summary like this:

```
---
score:          8
total_tokens_M:   499.6
num_steps:        50
```

## Logging results

When an iteration is done, log it to `results.tsv` (tab-separated, NOT comma-separated — commas break in descriptions).

The TSV has a header row and 4 columns:

```
commit	score	status	description
```

1. git commit hash (short, 7 chars)
2. score achieved (e.g. 7) — use 0 for crashes
4. status: `keep`, `discard`, or `crash`
5. short text description of what this iteration tried

Example:

```
commit	score	status	description
a1b2c3d	4	keep	baseline
b2c3d4e	5	keep	adjusted the methodology steps
c3d4e5f	3	discard	switch to previous steps
d4e5f6g	0	crash	image did not generate
```

## The experiment loop

The experiment runs on a dedicated branch (e.g. `sketchy/mar25` or `sketchy/mar25-style`).

LOOP UNTIL USER ABORTS:

1. Look at the git state: the current branch/commit we're on
2. Tune `sketch.md` with an experimental idea by directly hacking the prompt.
3. git commit
4. install the skill sketch.md in Claude Code. overwrite the previous version if one exists already.
4. Run the iteration: `/sketch concept-sketch.md output.png` (redirect everything — do NOT use tee or let output flood your context)
5. Read what's in output.png that the skill generated
6. If output.png is empty, the iteration crashed. Record a score of 0 in results.tsv file
7. If output.png is NOT empty, ask the user to rate the sketch on a scale of 1 to 10 (1 being worst, 10 being best). Record the rating as the score for this iteration in the results.tsv file. (NOTE: do not commit the results.tsv file, leave it untracked by git)
8. If score improved (higher), you "advance" the branch, keeping the git commit
9. If score is equal or worse, you git reset back to where you started

The idea is that you are a completely autonomous skill developer trying things out. If they work, keep. If they don't, discard. And you're advancing the branch so that you can iterate. If you feel like you're getting stuck in some way, you can rewind but you should probably do this very very sparingly (if ever).

**Crashes**: If an iteration crashes (e.g., empty output.png), use your judgment: If it's something dumb and easy to fix (e.g. a typo, a missing import), fix it and re-run. If the idea itself is fundamentally broken, just skip it, log "crash" as the status in the tsv, and move on.

**NEVER STOP**: Once the experiment loop has begun (after the initial setup), do NOT pause to ask the human if you should continue. Do NOT ask "should I keep going?" or "is this a good stopping point?". You can only ask the human to scoring, rest keep going. You are autonomous. If you run out of ideas, think harder — read papers on how to build agents and prompts. Re-read the in-scope files for new angles, try combining previous near-misses, try more radical changes. The loop runs until the human interrupts you, period.
