# Contributing

Thanks for taking an interest in PE4UAV. The project is research code, so reproducibility and clear scope matter more than hiding rough edges.

## Before changing code

1. Read `README.md`, `docs/EXPERIMENTS.md` and `docs/SECURITY_AND_SCOPE.md`.
2. Identify whether the change affects the SITL path, the local research benchmark, or both.
3. Keep the two result tracks separate unless the experiment design explicitly links them.
4. Add tests for protocol behaviour, especially malformed input, replay, wrong direction, state rollback, exhaustion and recovery paths where relevant.

## Local checks

```bash
python -m pip install -r requirements.txt
python -m pip install -r research_ready/requirements-research.txt
python -m compileall -q .
pytest -q
```

Live SITL checks should be run separately and documented with the ArduPilot revision, number of nodes, configuration, calibration and hardware-profile provenance.

## Code style

- Prefer small functions with one clear responsibility.
- Use explicit names for cryptographic state and experiment configuration.
- Validate external input at module boundaries.
- Keep protocol framing and parsing deterministic.
- Add type hints to new public functions where practical.
- Use Python's `logging` module for new long-running components instead of ad-hoc print output.
- Avoid logging secrets, private keys, plaintext payloads or sensitive telemetry.
- Keep comments focused on behaviour and non-obvious decisions.

## Results and data

Never hand-edit measured values to improve a graph or table. Store new runs in a separate output directory and retain configuration, environment details and hashes.

## Pull requests

Describe the research question, changed modules, tests run, environment, known limitations and any changes to claims or assumptions. Do not claim live SITL validation unless the swarm was actually launched and observed.
