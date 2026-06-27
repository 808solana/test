# Neuralwatt 200-Prompt Run — Plan

A living doc for running ~200 prompts through the Neuralwatt "answer engine" to measure token usage and drive the caching rate to zero.

## Goal
Feed ~200 distinct prompts through the local Neuralwatt tester UI (`neuralwatt_test.py`, model `glm-5.2`) to measure real token usage and **get the caching rate to zero**. Prompts are all different on purpose so nothing gets cached.

## The app
- Single Flask app at `http://localhost:3000` ("Neuralwatt Zero-Cache Tester").
- Sends each prompt to Neuralwatt and records tokens / cost / energy per request; SESSION STATS are cumulative server-side and persist for the life of the running server.
- Anti-cache measures already baked in: random nonce in the system prompt, random `seed`, temperature jitter, no-cache headers.

## How we're running it
- **Method: the slow, realistic GUI way.** Type the prompt into the UI → click **Send Request** → wait until the answer engine is fully done → only then enter the next one. No programmatic shortcuts — we want "the most realistic test."
- **Pacing:** send the next prompt as soon as the current one is confirmed complete. No artificial delay.

## Prompts
- Delivered as a **JSON file** in the repo: an array of strings, e.g. `["prompt one", "prompt two", ...]`.
- All **one-line** prompts, all **distinct**. (Filename TBD — to be dropped in by the user.)

## Caching measurement
- **Not** implemented in the Python UI — by design. The user reads the full caching rate / usage directly on the **Neuralwatt dashboard**. No code changes needed.

## Proof / deliverable
- **End result only.** No long continuous recording ("it's kind of a boring process to watch you put two hundred prompts in," and the file would be huge).
- At the end: a short screenshot/video of the final SESSION STATS (showing `Total Requests = 200`) and the REQUEST HISTORY.
- A live/short clip is fine, but the priority is clean end-of-run proof.

## Error handling
- **Stop immediately** on any error. Capture a screenshot/video of the error and **report the exact prompt number reached** (e.g. "failed at #57"). The error screen counts as "the end result."
- User fixes the issue, then we resume / restart.

## Status / next step
- Environment is set up; app runs; a single test chat already succeeded against Neuralwatt.
- **Blocked on input:** waiting for the prompts JSON file to be dropped into the repo. Once it's here, the run starts.
