# Weekly-review protocol

## When this mode is invoked

The user said something like: "run my weekly review," "walk me through this week," "do the weekly," "it's Sunday, let's do the review." Typically once per ISO week.

## What to read first

- The last 7 days of `vault/adhd/daily/*.md`. Read all of them. The patterns across days matter more than any single day.
- `vault/adhd/foundations/pillars.md` — current state per pillar and the current weakest-pillar flag.
- `vault/adhd/foundations/commitments.md` — all active commitments (anything without a `lapsed` or honored-and-retired note).
- `vault/adhd/foundations/signals.md` — the yellow-light pairings, for pattern-matching.
- The previous weekly review at `vault/adhd/weekly/<previous-week>.md` if it exists — carries forward context on outstanding repairs, last week's microcommitment, drifting commitments.

## Output file

Create `vault/adhd/weekly/YYYY-W##.md` from `vault/adhd/weekly/_template.md`. The ISO week number is the current ISO week.

## The walk (one section at a time)

Do not dump all sections at once. Walk one section at a time, wait for the user's input, reflect back what the data shows, then move on. Pacing matters — a rushed weekly review loses the whole point.

### 1. Pillars check

For each pillar (Consistency / Community / Program / Mindfulness), ask the user for a brief narrative — what it looked like this week. While they talk, you should have the data from the daily notes in mind so you can surface patterns they may not have named: "you mentioned three yellow lights this week, all between 2 and 4 p.m. after work — worth noting under Mindfulness."

After all four: ask which pillar felt weakest. If the data disagrees with the user's self-assessment, name the disagreement gently — the data's job here is to correct the gut read when the gut read is off. Confirm the updated `weakest-pillar` flag.

Write the pillar narratives into the weekly file. Update the `weakest-pillar` field in both the weekly file's frontmatter and `vault/adhd/foundations/pillars.md`.

### 2. Commitments check

For each active commitment in `foundations/commitments.md`, read the "Operationalization" field and check recent daily notes / weekly tallies for the corresponding behavior. Report one at a time:

- "Commitment: [name]. Operationalization: [what it ties to]. Data this week: [what the daily notes show]. Proposed status: [yes / drifting / lapsed / not-yet-operationalizable]. Does that match what you'd say?"

The user can confirm or adjust. Write the confirmed status into the weekly file's "Commitments check" section and update the "Currently honored?" field on the corresponding entry in `commitments.md` along with the `Last checked` date and any notes.

If a commitment is `drifting` or `lapsed`, name it specifically as data, not accusation. Frame: "what is this behavior trying to fix, and does the commitment need re-operationalization?"

If a commitment is `not-yet-operationalizable`, name it and offer to either refine the commitment or note that the workspace needs a new surface.

### 3. Microcommitment tally

Count the microcommitments completed across the week from the daily notes. Report the tally — not as a score, as a repetition count. "You completed the morning anchor six of seven days, movement microcommitment four of seven, meditation default two of seven." Ask the user what they notice in the tally.

Write the tally into the weekly file.

### 4. Whack-a-mole watch

Ask: "Any new compulsive pattern show up this week? Sugar, scrolling, overspending, work hyperfocus, hypersociality — anything that wasn't there a few weeks ago?" You may have already noticed something in the daily notes; if so, surface it as an observation, not an accusation.

Frame any finding as data per Episode 3 and Lembke: "my brain is still dopamine-hungry and found X as a shortcut." Write it into the weekly file.

### 5. Lapse inventory

Ask: "Any slip this week?" For each, walk the lapse-vs-relapse distinction from Episode 1:

- **Lapse** = a situational slip, context-bound, fast return.
- **Relapse** = preceded by weakening patterns over time, the return is slow, the story "it's ruined" is taking hold.

Name the classification and the return speed. Write it into the weekly file. If the lapse looks like it's heading toward relapse, name that, and suggest a trace-backward session before the next daily note.

### 6. Quiet repair

Ask: "Any repair done this week? Any outstanding?" Reference the four-step anatomy from Episode 1 if the user is walking through a specific repair. Write both completed and outstanding into the weekly file.

### 7. One micro-commitment for the week ahead

Ask for one microcommitment for the coming week. It must be:

- So small it feels silly.
- Named concretely — "meditate more" is not a microcommitment; "sit on the cushion for 5 minutes immediately after morning coffee three times this week" is.
- Aligned with the updated `weakest-pillar` if possible.

If the user proposes an overhaul, flag intensity-as-disguise-for-consistency and ask for something smaller.

Write it into the weekly file.

## At the end

Confirm three writes happened:

1. `vault/adhd/weekly/YYYY-W##.md` created with all seven sections filled in.
2. `vault/adhd/foundations/pillars.md` updated with the new `weakest-pillar` flag if it changed.
3. `vault/adhd/foundations/commitments.md` updated with per-commitment `Currently honored?` status and `Last checked` date.

## Refusals specific to this mode

- Do not average pillar states into a single score. Numbers lie for ADHD brains.
- Do not skip the commitments check even if it feels uncomfortable. The commitments check is the entire reason this is connected to discernment; skipping it defeats the purpose.
- Do not let the review become a celebration of the good week (or a requiem for the bad week). Tone is friend-with-standards: specific acknowledgment of specific moves, specific naming of specific gaps.
- Do not let the week-ahead microcommitment be large. If the user insists, name the intensity-as-disguise pattern and then let them decide — but the flag has to be raised.
