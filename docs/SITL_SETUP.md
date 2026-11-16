# ArduCopter SITL setup

The root-level swarm workflow targets Ubuntu/WSL2. It assumes access to an ArduPilot source tree and a system capable of building ArduCopter SITL.

## Environment variables

| Variable | Default | Purpose |
|---|---|---|
| `ARDUPILOT_DIR` | `$HOME/ardupilot` | ArduPilot checkout |
| `ARDUPILOT_VENV` | `$HOME/ardupilot-venv` | Python virtual environment |
| `SIM_VEHICLE` | `$ARDUPILOT_DIR/Tools/autotest/sim_vehicle.py` | SITL launcher script |
| `GUI` | `1` | Request GUI for the first vehicle when a display exists |
| `SPEEDUP` | `1` | SITL speed-up factor |
| `TMUX_SESSION` | `fanet` | tmux session name |
| `DURATION` | script-specific | Experiment duration |
| `ATTACKS` | script-specific | Whether to enable attack scenarios |

The swarm script accepts the number of nodes as its first positional argument. The checked-in network ports and SYSID mapping are documented in the script; update dependent proxy/calibration configuration together when changing them.

## Setup and launch

```bash
./setup_fanet_sim.sh
source ~/ardupilot-venv/bin/activate
./launch_sitl_swarm.sh 5
```

The setup script may clone ArduPilot, initialize submodules, install prerequisites and build SITL. Review it before running on a machine with existing ArduPilot changes.

## Calibrate before measuring

Do not treat example network or power values as measured values. Generate link calibration from a live run and create a hardware profile using documented measurements or datasheet-backed inputs. Retain calibration metadata with each experiment.

## Troubleshooting

- **`sim_vehicle.py` missing:** set `ARDUPILOT_DIR` to a complete ArduPilot checkout.
- **SITL executable missing:** complete the ArduPilot build using its documented setup process.
- **No heartbeat:** inspect tmux windows, UDP port conflicts and per-node SITL stdout logs.
- **No GUI:** set `GUI=0` for headless operation.
- **Missing link profile:** run `sitl_calibrator.py` while the swarm is active.
- **Missing hardware profile:** generate one with `hw_profile.py`; do not fill unknown energy values with guesses.
