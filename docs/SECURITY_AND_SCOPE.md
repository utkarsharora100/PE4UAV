# Security, safety and scope

PE4UAV is experimental research software. It is not a flight-control system, certified avionics component, production key-management service or approved cryptographic implementation.

## Protocol language

- The `research_ready` EC-seeded design is an authenticated EC-seeded GGM-style puncturable-key transport prototype. Do not describe it as a proven public-key puncturable-encryption scheme.
- The root-level recovery and ratchet modules are project implementations. Do not claim complete conformance to an external protocol specification unless independently established.
- A property-test table or passing test suite is not a formal proof of security.

## Operational safety

- Run SITL experiments in an isolated development environment.
- Do not connect the prototype to real aircraft or safety-critical control links.
- Do not place real identity private keys, production secrets or sensitive flight logs in this repository.
- Treat calibration and hardware-profile inputs as experiment configuration. Record their source and measurement method.
- Inspect generated command lines and target paths before running scripts that clone, build or launch external software.

## Interpreting evidence

A benchmark supports claims only about the workload and environment it actually measured. Local Python/OpenSSL timing does not establish radio performance, packet delivery, embedded energy use, fleet-scale reliability, resistance to all attacks or flight readiness.

Before publication, obtain independent protocol review, repeat measurements on stated hardware, validate network behaviour under controlled conditions and document assumptions and exclusions.
