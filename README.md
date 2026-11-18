# Promo Finder (Kuku AI Generalist assignment, Problem 1)

Finds 20 to 60 second promo clips in microdrama screenplays. Runs entirely in the browser: one HTML file, no server, no install.

## Run it

1. Download `Promo_Finder.html` and open it in Chrome or Edge from your computer. Do not open it from a hosted preview, because browsers block the calls to the model APIs there.
2. Choose the episode `.txt` files (one episode or the whole show).
3. Paste a Google AI Studio key and a model name in "Primary model". Optionally add a fallback (any OpenAI-compatible API, such as Groq) and keep "Judge uses a different model" selected.
4. Press "Run episodes not done yet". Use "Download results (.json)" and "Download CSV" at any time. "Load saved results…" restores a saved run so you can continue it.

Keys stay in the page's memory and go only to their own provider. Never commit a key.

## How it works

| Step | Done by | What it does |
|---|---|---|
| Parse | code | Reads each screenplay into blocks with IDs and timestamps; warns on odd files |
| Propose | LLM | Picks start and end block IDs for up to 4 candidates per episode. It never writes timestamps |
| Auto-fix | code | Moves a start or end off a camera/sound line, fixes an off-by-one block ID, adds lines to a clip that is a few seconds short |
| Check | code | Block IDs exist, quotes match the text, first and last blocks are dialogue or action, length is 20 to 60s |
| Repair | LLM | One follow-up call for rejected candidates, with the exact errors |
| Judge | a different LLM | Scores hook, cliffhanger and stands-alone 1 to 5, checks the proposer's claims against the clip, may suggest a trim |
| Verdict and rank | code | Post / fix / drop from the scores; ranking is a scoring rule, not a model opinion |

The model call is one function (`callModel`). Swapping a model or provider means changing the settings, not the code.

## Skill files (the prompts)

`promo_criteria.md`, `candidate_proposer.md` and `promo_judge.md` are built into the page. The "taste rules" section of `promo_criteria.md` is for the editor to fill in.

## Known limits

See the solution doc. In short: the judge is generous on cliffhangers, descriptions from the proposer often drift from the clip, a weaker proposer model needs the code-side fixes, and nothing here has been checked against how the clips actually perform.

## Confidentiality

The screenplays are Kuku's and are not included in this repo.
