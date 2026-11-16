# PE4UAV

[![Tests](https://github.com/utkarsharora100/PE4UAV/actions/workflows/tests.yml/badge.svg)](https://github.com/utkarsharora100/PE4UAV/actions/workflows/tests.yml)

PE4UAV is an experimental framework for evaluating authenticated cryptographic transport and key-management approaches for MAVLink telemetry in a flying ad-hoc network (FANET). It combines protocol implementations, a five-node ArduCopter Software-in-the-Loop (SITL) workflow, telemetry and resource collection, security tests, and a separate reproducible software microbenchmark.

> **Research software, not flight software.** The included protocols and scripts are experimental. Do not deploy them on real aircraft or use the reported measurements as evidence of flight readiness or production-grade cryptographic security. Read [`docs/SECURITY_AND_SCOPE.md`](docs/SECURITY_AND_SCOPE.md) before interpreting results.

## What is in this repository?

| Area | Purpose |
|---|---|
| Root Python modules | FANET experiment orchestration, proxy, telemetry, crypto analysis, recovery and resource collection |
| `mavlink/` | Custom MAVLink message definition used by the project |
| `tests/` | Security, protocol, proxy and property tests for the main experiment code |
| `research_ready/` | Isolated authenticated EC-seeded puncturable-key transport prototype and microbenchmark |
| `research_ready/tests/` | Tests for the isolated research prototype |
| `docs/` | Setup, experiment workflow, result interpretation and contribution guidance |
| `.github/workflows/` | Continuous integration for the Python test suites |

This repository contains two related but distinct evaluation tracks:

1. **SITL swarm experiments** use ArduCopter and MAVLink to evaluate proxy behaviour, network delay, telemetry, resource use and attack/recovery scenarios. They require a local ArduPilot build and calibrated environment.
2. **Research microbenchmarks** in `research_ready/` compare local software costs for selected protected-record designs. These are in-process measurements, not wireless-network or drone-hardware measurements.

Their outputs must not be merged or described as one experiment. See [`docs/EXPERIMENTS.md`](docs/EXPERIMENTS.md).

## Quick start: research microbenchmark

The research benchmark does not require ArduPilot or a running SITL swarm. The included package documents Python 3.11–3.13; Python 3.13.5 was used for the recorded run.

```bash
git clone https://github.com/utkarsharora100/PE4UAV.git
cd PE4UAV
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\\Scripts\\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r research_ready/requirements-research.txt
pytest -q research_ready/tests
python research_ready/mavlink_microbench.py --packets 1000 --repeats 7 --ratchet-every 250 --depth 16 --seed 20261016 --output-dir research_ready/results
python research_ready/analyze_results.py --results-dir research_ready/results --figures-dir research_ready/figures
```

For a quick smoke run:

```bash
python research_ready/mavlink_microbench.py --packets 24 --repeats 2 --ratchet-every 8 --depth 8 --output-dir /tmp/pe4uav-smoke-results
python research_ready/analyze_results.py --results-dir /tmp/pe4uav-smoke-results --figures-dir /tmp/pe4uav-smoke-figures
```

See [`research_ready/README.md`](research_ready/README.md) for workload details, assumptions and full reproduction instructions.

## Quick start: security tests

```bash
python -m pip install -r requirements.txt
python security_tests.py
pytest -q tests
```

The full test suite can also be run with `pytest -q`. Live SITL checks require the simulator and should be treated separately from unit tests.

## Run the five-node ArduCopter SITL experiment

The full SITL path targets Ubuntu or WSL2 and requires ArduPilot, its submodules and build prerequisites, `tmux`, MAVProxy, and a working Python environment.

```bash
chmod +x setup_fanet_sim.sh launch_sitl_swarm.sh run_full_experiment.sh
./setup_fanet_sim.sh
source ~/ardupilot-venv/bin/activate
./launch_sitl_swarm.sh
```

Attach to the session or stop it:

```bash
tmux attach -t fanet
tmux kill-session -t fanet
```

Calibrate while SITL is running:

```bash
python sitl_calibrator.py --node1-port 14650 --node2-port 14651 --capture-secs 20 --n-pings 100 --output fanet_links.json
```

Generate a hardware profile using measured or sourced values:

```bash
python hw_profile.py generate --calib fanet_links.json --cpu-active-w 0.50 --cpu-idle-w 0.18 --radio-tx-w 0.33 --battery-wh 7.40 --description \"Replace with the actual hardware profile\" --output hw_profile.json
```

The power values shown above are examples from the original setup guide, not measured facts. Replace them before drawing energy conclusions. See [`docs/SITL_SETUP.md`](docs/SITL_SETUP.md) for configuration and troubleshooting.

## Security and research limitations

- The EC-seeded design in `research_ready/` is an **authenticated EC-seeded GGM-style puncturable-key transport prototype**. It is not claimed to be a proven public-key puncturable-encryption construction.
- The project ratchet implementation is an experimental baseline. It is not claimed to implement or formally conform to the complete Signal Double Ratchet specification.
- The local microbenchmark does not measure UDP, radio airtime, packet loss, wireless contention, embedded CPU/battery performance or fleet-scale behaviour.
- Passing tests do not establish formal security, production readiness or flight safety.

Read [`docs/SECURITY_AND_SCOPE.md`](docs/SECURITY_AND_SCOPE.md) and [`research_ready/SECURITY_MODEL.md`](research_ready/SECURITY_MODEL.md) before reusing code or citing results.

## Development

```bash
python -m pip install -r requirements.txt
python -m pip install -r research_ready/requirements-research.txt
python -m compileall -q .
pytest -q
```

Please keep protocol changes covered by negative tests, document changes to experimental assumptions, and avoid overwriting recorded outputs by hand. See [`CONTRIBUTING.md`](CONTRIBUTING.md).

## License

No license file was present in the supplied project archive. Until the repository owner adds one, all rights remain reserved and reuse permissions are not explicitly granted.
