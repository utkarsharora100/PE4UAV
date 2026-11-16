# Experiment tracks

PE4UAV currently holds two separate tracks. They share a research topic, but use different execution environments and measure different things.

## Track A — FANET / ArduCopter SITL

The root-level scripts and modules orchestrate a multi-vehicle ArduCopter simulation, MAVLink proxying, cryptographic operations, calibrated network delay, telemetry collection and attack/recovery scenarios.

Typical outputs include proxy event and crypto timing CSV files (`results_<ALGORITHM>.csv`), mission telemetry CSV files (`mission_<ALGORITHM>.csv`), calibrated link and hardware-profile JSON files, and a generated benchmark report.

A valid run depends on live SITL vehicles, expected UDP endpoints, a calibrated link profile and an appropriate hardware profile. Static output files alone do not prove that a fresh run reproduced the experiment.

## Track B — local research microbenchmark

`research_ready/` is a self-contained software benchmark. It generates MAVLink v2 HEARTBEAT frames, checks their CRC, groups valid frames into protected records and times local cryptographic operations.

The comparison includes a static AES-GCM record, the project's ratchet baseline and the authenticated EC-seeded GGM-style puncturable-key transport prototype. Their key-establishment assumptions differ; consult `research_ready/EXPERIMENT_METHOD.md` before comparing results.

The benchmark records software encrypt/decrypt timings, framing bytes, local initialization/handshake costs and derived trial summaries. It does not simulate wireless transport.

## Reporting rule

Do not combine results from the two tracks into a single table unless measurement definitions, workload, machine, environment and provenance are explicitly reconciled. Local cryptographic latency is not end-to-end FANET latency, and host process measurements are not drone energy measurements.
