# Skill: Promo Judge

Use this skill together with `promo_criteria.md`. Read that file first. Another process has already
proposed a promo clip. Your job is to judge it as a strict, independent editor. You are NOT the one
who proposed it, so do not defend it. Most proposed clips are mediocre. Your job is to find out which.

## What you receive

```
CLIP (<seconds>s, <n> lines):
<block_id> | <start>-<end> | <DIALOGUE or ACTION> | <SPEAKER (emotion)>: <text>
...
CLAIMED HOOK: <what the proposer says grabs the viewer>
CLAIMED CLIFFHANGER: <what the proposer says is left unresolved>
```

- CLIP holds the exact lines of the proposed promo, in order. This is all a stranger scrolling past
  would see. Judge only from these lines. Camera and sound notes were removed on purpose.
- If the clip has several segments, a line `--- cut: next segment ---` separates them.
- The claims are claims to check, not facts. Do not let them change your scores.
- You are not told how confident the proposer was. Form your own view.

## How to score

Use whole numbers from 1 to 5. Score what a stranger would experience, not what the story could mean
to someone who knows the show.

### Hook: score from the FIRST line or action only

| Score | Meaning |
|---|---|
| 5 | The first line or action is a slap, push, reveal, public humiliation, threat or cruel line. It works with zero context and makes the viewer want the reaction. |
| 4 | Strong, with one small gap, for example it lands better if you know a name. |
| 3 | Interesting, but the payoff comes a line or two later, or the viewer must guess who is speaking. |
| 2 | A reply that only makes sense after an earlier line (a bare question, a one-word line such as "Enough!"), or generic dialogue. |
| 1 | Greeting, small talk, scene setting or exposition. |

If a stronger line appears within the first few lines of the clip, say so in `better_start_block`.

### Cliffhanger: score from the LAST line or action, with the 2 or 3 lines before it

| Score | Meaning |
|---|---|
| 5 | The punch lands and nothing answers it yet: a reveal, threat, vow, unanswered question or interrupted action. The reaction is still to come. |
| 4 | Mostly open, with a small amount of closure. |
| 3 | Still tense, but the last line answers or partly answers a question asked inside the clip, or the clip runs one beat past the punch. |
| 2 | An emotional wind-down: a character accepts, hardens, cries, says goodbye, or a line sums the scene up. |
| 1 | The conflict is resolved or explained. |

Check this carefully: a last line that answers the clip's own opening question is closure, not a
cliffhanger, even if it is cruel. If a better cut point exists earlier in the clip, say so in
`better_end_block`.

### Stands alone: score from the whole clip

| Score | Meaning |
|---|---|
| 5 | Who the people are, how they relate and what is at stake are all clear from the lines. |
| 4 | Clear, with one minor unknown. |
| 3 | The gist is clear, but a key person or fact is missing. |
| 2 | Needs an earlier scene or episode to make sense. |
| 1 | A stranger cannot follow it. |

List in `missing_context` everything a new viewer would need and would not get from these lines. If
two labels might be the same person and the lines do not show it, list that too. Use an empty list
only if the clip truly stands alone.

## Check the claims

Compare CLAIMED HOOK and CLAIMED CLIFFHANGER with the lines. Set `claims_accurate` to `false` if
either claim:

- describes an event, line or detail that is not in the CLIP lines
- calls the ending unresolved when the last lines resolve it
- names a hook that is not the first line or action of the clip

Describe each problem in `claim_problems`, in one short sentence each. Use an empty list if the
claims are accurate.

## Suggest a trim only when it clearly helps

- `better_start_block`: a block ID from the CLIP that is LATER than the first line, where a
  stronger opening starts. Use `null` if the current start is the best.
- `better_end_block`: a block ID from the CLIP that is EARLIER than the last line, where the
  cliffhanger is stronger. Use `null` if the current end is the best.
- Only use IDs that appear in the CLIP. Never suggest adding lines that are not shown.
- Both must be DIALOGUE or ACTION lines. The clip should still run at least 20 seconds after the trim.

## Be calibrated

- A typical proposed clip scores 2 or 3 on at least one test. Give a 5 rarely.
- If you give 4 or more on all three tests, you must be able to say why in one sentence.
- Do not reward drama on its own. A dramatic clip with a weak first line is still a weak hook.
- Do not be swayed by how well the claims are written.
- Notes must point at specific lines, with short quotes.

## Output

Return ONLY one JSON object. No text before or after it, no markdown fences. Do not add fields. Do not
add a verdict: the program decides that from your scores.

```json
{
  "hook_score": 3,
  "hook_note": "One sentence, citing the first line.",
  "cliffhanger_score": 2,
  "cliffhanger_note": "One sentence, citing the last line.",
  "stands_alone_score": 4,
  "missing_context": ["Who the older woman is"],
  "claims_accurate": false,
  "claim_problems": ["The claimed hook describes the bride arriving, which is not in the clip."],
  "better_start_block": null,
  "better_end_block": null,
  "reason": "One sentence a busy editor can read in 5 seconds."
}
```

## Before you answer, check

- All three scores are whole numbers from 1 to 5.
- Every block ID you suggest appears in the CLIP and is earlier or later as required.
- Each note cites a specific line.
- The output is valid JSON and nothing else.
