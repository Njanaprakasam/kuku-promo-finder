# Skill: Candidate Proposer

Use this skill together with `promo_criteria.md`. Read that file first. This file says exactly how to
read one episode and what to return.

## Your job

Read ONE episode of a microdrama screenplay and propose up to 4 promo candidates from it. For each
candidate, choose where it starts and where it ends, using block IDs. You never write timestamps.
The program turns your block IDs into timestamps.

## What you receive

The user message contains one episode in this compact format:

```
EPISODE: <number>
DURATION: <seconds>
LOGLINE: <one sentence>
CONTINUITY: <one or two sentences>

<block_id> | <start>-<end> | <TYPE> | <SPEAKER (emotion)>: <text>
```

- `<start>` and `<end>` are seconds from the start of the episode, for example `11.3-13.1`.
- `<TYPE>` is `DIALOGUE`, `ACTION`, `CAMERA` or `SOUND_FX`.
- Dialogue lines carry the speaker label and an emotion in brackets.
- Camera and sound notes are context only. They describe the shot, not the story.
- The same person may appear under several labels, such as a role and a name. If the text lets you
  tell two labels are one person, treat them as one. If it does not, do not guess.

## Rules

1. **Block IDs only.** `start_block` and `end_block` must be copied exactly from the input. Never
   invent an ID, a time or a line.
2. **Start and end on DIALOGUE or ACTION blocks.** Never start or end a promo on a CAMERA or
   SOUND_FX block.
3. **Length.** The promo runs from the start time of its first block to the end time of its last
   block. Add up the segments if there are several. The total must be between 20 and 60 seconds. Aim
   for 30 to 45. Check this using the times in the input before you answer.
4. **Order and shape.** Segments are in episode order and do not overlap. Use one segment by
   default. Use 2 or 3 only for a `multi_segment_teaser`, and each segment must be at least 5 seconds.
5. **Pick start and end separately.** Find the strongest opening moment and the strongest unresolved
   ending, then check the stretch between them holds together and fits the length limit.
6. **Distinct candidates.** No two candidates may share more than a quarter of their time.
7. **Quality over quantity.** Return 0 to 4 candidates. Fewer is better than weak. If nothing in the
   episode passes the three tests in `promo_criteria.md`, return an empty list and say why.
8. **Quote exactly.** `hook_quote` and `cliffhanger_quote` must be copied word for word from the
   text of the first and last block you chose. The program checks this.
9. **Be honest about context.** List in `needs_context` anything a new viewer would need and would
   not get from the clip. Use an empty list only if the clip truly stands alone.
10. **Confidence.** Use `high` only if all three tests clearly pass. Use `low` if one is shaky, and
    say which in `why_it_works`.

## Output

Return ONLY one JSON object. No text before or after it, no markdown fences.

```json
{
  "episode": 1,
  "candidates": [
    {
      "segments": [
        { "start_block": "e1-s1-b008", "end_block": "e1-s1-b030" }
      ],
      "promo_type": "confrontation",
      "hook": "One sentence: what grabs the viewer in the first 3 seconds.",
      "hook_quote": "Exact text of the first dialogue or action block.",
      "cliffhanger": "One sentence: what is left unresolved at the cut.",
      "cliffhanger_quote": "Exact text of the last dialogue or action block.",
      "why_it_works": "One sentence a busy editor can read in 5 seconds.",
      "needs_context": ["Who the older woman is"],
      "confidence": "medium"
    }
  ],
  "no_good_promo_reason": null
}
```

Field rules:

- `promo_type` is one of: `cold_open_shock`, `humiliation_to_defiance`, `confrontation`,
  `reveal_twist`, `multi_segment_teaser`.
- `confidence` is one of: `high`, `medium`, `low`.
- `no_good_promo_reason` is `null` when there are candidates. When `candidates` is empty, it is one
  sentence explaining why the episode has no strong promo.
- Do not add fields. Do not add comments.

## Before you answer, check

- Every block ID appears in the input.
- First and last blocks are DIALOGUE or ACTION.
- Total length is 20 to 60 seconds by the input times.
- Both quotes are exact.
- The ending is unresolved, not wrapped up.
- The output is valid JSON and nothing else.

## Length: the most common mistake

The program automatically rejects any candidate under 20 seconds or over 60 seconds. Do the arithmetic for every candidate before you answer:

length = end time of the LAST block minus start time of the FIRST block

Example: first block `11.3-13.1`, last block `38.0-41.6`. The length is 41.6 - 11.3 = 30.3 seconds. Good.

A single punchy exchange is often only 5 to 15 seconds, which is too short. To reach 30 to 45 seconds, start a few lines earlier, at the line that sets up the moment, but only if that earlier line is itself interesting. Do not pad the end: the clip must still stop right after the punch.

If you cannot reach 20 seconds without ruining the hook or the cliffhanger, leave that moment out.

## Quotes

`hook_quote` and `cliffhanger_quote` are the text of the line only. Do not include the speaker name or the emotion in brackets.
