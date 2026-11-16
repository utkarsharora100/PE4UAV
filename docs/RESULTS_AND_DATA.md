# Results and data handling

## Included reference material

The repository retains compact aggregated research benchmark outputs, environment metadata, manifests, summary tables and figures under `research_ready/results/` and `research_ready/figures/`. These are reference artifacts from the supplied archive. They are not a fresh run from this checkout.

## Intentionally excluded from Git

The supplied archive also contained large machine-generated artifacts, including raw MAVLink telemetry logs, multi-megabyte per-algorithm mission and proxy CSV files, a raw packet-latency capture, binary EEPROM data and terrain/build data. These files are not needed to install or run the source and can make routine clones unnecessarily large.

They are therefore not copied into this repository snapshot. Keep original copies in durable experiment storage. For a publication-grade release, provide a versioned data archive, checksums, a data dictionary and acquisition provenance.

## Regenerating outputs

Use the commands in the root README and `research_ready/README.md`. Write exploratory runs to a separate output directory so tracked reference results are not silently overwritten.

Do not edit raw or derived measurement values by hand. Change code or configuration, rerun the experiment, and preserve complete run metadata.
