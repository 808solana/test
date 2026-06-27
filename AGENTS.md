# AGENTS.md

## Cursor Cloud specific instructions

### Overview
This repo is a single-file Python/Flask app: `neuralwatt_test.py` ("Neuralwatt Zero-Cache Tester"). It serves a small web UI on port `3000` that sends prompts to the Neuralwatt API (OpenAI-compatible, model `glm-5.2`) and reports per-request token usage, cost, and energy.

### Dependencies
Runtime deps are `flask`, `openai`, and `requests` (installed by the startup update script). There is no lockfile, `requirements.txt`, or test/lint framework.

### Run
- Start: `python3 neuralwatt_test.py` (serves at http://localhost:3000).
- Debug mode is on, so the Werkzeug reloader restarts the process on file save and prints the banner twice — this is expected, not an error.
- Endpoints: `GET /` (UI), `POST /chat` (`{"prompt": "..."}`), `GET /stats`, `POST /reset`.

### Lint / test / build
- No build step and no lint or test suite exist. For a quick sanity check use `python3 -m py_compile neuralwatt_test.py`.

### Gotchas
- `/chat` calls the external Neuralwatt API (`https://api.neuralwatt.com/v1`) and requires outbound network access. The API key is currently hardcoded in `neuralwatt_test.py` (`NEURALWATT_API_KEY`); the code also reads `NEURALWATT_API_KEY` from the env per its docstring, so prefer setting that env var if the hardcoded key stops working.
- Stats are in-memory and reset on restart.
