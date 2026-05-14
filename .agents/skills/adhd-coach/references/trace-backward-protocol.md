# Trace-backward protocol

## When this mode is invoked

The user said something like: "trace backward," "help me understand what happened with X," "walk me back from [aftermath]," "why did that happen," "can we reverse-engineer last night." Or a weekly review surfaced a lapse and the user wants to understand the sequence.

## What to do first

Read `vault/adhd/foundations/signals.md` (for named signals) and `vault/adhd/foundations/pillars.md` (for the current weakest-pillar context). If the aftermath is from a specific day, read `vault/adhd/daily/<that-date>.md` — the contemporaneous notes are often more useful than the user's memory.

## The walk (reverse order, deliberately)

Per Episode 3, work **backward** from the consequence to the trigger. Do not let the user start with "so the thing that triggered it was…" — that's the theory they've already built. The point is to find what they haven't yet seen.

1. **Start at the aftermath.** Ask: "What happened? What was the consequence?" Get it concrete — the actual behavior, not the feeling about the behavior.
2. **One step back: the action.** "What was the last action you took before the consequence? Walk me through it — what specifically did you do?"
3. **One step back: the urge narrative.** "What was the story in your head at that moment? What did the urge tell you?" Per Lembke, urges disguise themselves as logic — "I deserve this," "just this once," "this will help me calm down." Ask for the verbatim phrasing if possible.
4. **One step back: the trigger moment.** "What happened right before the urge surfaced? What specifically triggered it — a feeling, a conversation, an event, an internal state?"
5. **One step back: the signal.** "What were you feeling physically or emotionally in the minutes leading up to the trigger? Was there a yellow-light signal you recognize from `signals.md`? If so, was there a moment you could have caught it?"

At each step, push for specifics. Vague answers ("I was stressed") are the theory; concrete answers ("I was sitting at my desk, 2:17 p.m., second meeting had just ended, I was hungry and hadn't eaten lunch") are the data.

## What to surface

After the walk, name two things in plain language:

1. **Where the gap collapsed.** Per Episode 3, ADHD brains move directly from trigger to choice, eliminating the natural pause where reflection happens. Name the specific place in this particular sequence where the gap closed. "The gap closed between the hunger-at-2:17 signal and the first click on the site — there was no pause inserted anywhere."
2. **The last moment a pause was possible.** Be specific. "The last moment the pause was possible was when you noticed you hadn't eaten but chose to push through the next meeting instead of stopping for food. That's the decision point."

Not two moralizing sentences — two diagnostic observations.

## After the trace

Ask one question: "What's one concrete thing you could change in the week ahead so this specific sequence has a pause in it?" Aim for a microcommitment-sized answer, not a life-overhaul answer. If the user proposes an overhaul, flag the intensity-as-disguise pattern and ask for something smaller.

If the resulting microcommitment looks like it should be tracked, offer to add it to `vault/adhd/foundations/commitments.md` (if it's a real commitment the user wants to be held to) or to the week-ahead microcommitment in the next weekly review.

## Offer to promote to an urge file

At the end, offer to create `vault/adhd/urges/YYYY-MM-DD-<slug>.md` with the reconstruction, in the format documented in `vault/adhd/urges/_README.md`. Ask the user for the slug.

If the user says yes: write the file with the five sections (Trigger / The story the urge told me / What it was trying to fix / What I did / What I noticed after). Include the gap-collapsed observation and the proposed microcommitment.

## Refusals specific to this mode

- Do not let the user start with the trigger. Reverse order is the whole point.
- Do not accept "I was stressed" or "I was tired" as a signal. Those are categories. Push for the specific physical or emotional sensation the signal file names or should name.
- Do not turn this into a therapy session. If the aftermath surfaces material the user wants to work on more deeply — trauma, relational wound, a grief that was underneath the craving — name it and route to the human layer (therapist, discernment coach if it intersects with a disclosure question).
- Do not moralize. Pleas do not rewrite the past. Accurate description does.
