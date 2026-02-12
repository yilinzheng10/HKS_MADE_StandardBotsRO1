## Purpose
This file provides concise, actionable instructions for AI coding agents working on the ros2-realtime-api repository.

## Big picture
- **Goal:** tools and Python samples to interact with Standard Bots via ROS2 over CycloneDDS. The repo provides small client scripts (under `src/`) that discover a robot, subscribe to hardware topics, and publish pose commands.
- **Runtime model:** code runs inside a Docker image (built by `./build.sh`) that configures ROS_DOMAIN_ID=1 and mounts the repo for iterative development. Networking is provided via `cyclonedds.xml` which must be configured to use the host network adapter on the same LAN as the robot.

## Major components & entry points
- `README.md` — run/build commands and examples. See run examples (build/run/run_shell).
- `src/detect_bot_id.py` — discovers the robot by scanning topics and matching `/([^/]*)/ro1/hardware/joint_state` (used by other scripts).
- `src/read_joint_states.py` — subscribes to `/<BOT_ID>/ro1/hardware/joint_state`. Uses QoS BEST_EFFORT to match hardware publisher.
- `src/write_poses.py` — subscribes to pose, publishes to `/.../pose/write`, and shows SDK usage: it requires `ROBOT_URL` and `ROBOT_TOKEN` (or CLI args) and toggles robot brakes and ROS control state via the `standardbots` SDK.

## Key workflows & commands (concrete)
- Build the development Docker image: `./build.sh`.
- Run a default example (auto-detects bot): `./run.sh`.
- Run a specific script inside the container: `./run.sh python3 ./src/read_joint_states.py`.
- Launch an interactive shell inside the dev container: `./run_shell.sh`.
- When using `write_poses.py` provide creds via CLI or env: `--url`, `--token` or set `ROBOT_URL`/`ROBOT_TOKEN` in environment.

## Project-specific conventions & patterns
- Network adapter must be set in `cyclonedds.xml` (<NetworkInterface name="..."/>) to the interface on the robot's LAN — this is a required manual step before building the image.
- The project uses ROS2 discovery for the bot id: topic naming convention is `/ <BOT_ID> /ro1/...`. Prefer reusing `detect_bot_id()` from `src/detect_bot_id.py` when adding tools.
- QoS: hardware publishers expect BEST_EFFORT reliability and small history (`depth=10`). New subscribers should mirror this QoS to avoid missed messages (see `src/read_joint_states.py` and `src/write_poses.py`).
- SDK usage: `standardbots.StandardBotsRobot` is used to switch brakes and ROS control state. Code assumes `with sdk.connection():` context manager and `.ok()` style results — follow existing pattern when adding SDK calls.

## Integration & external dependencies
- Docker, ROS2, and CycloneDDS are required for local execution. Docker is used to create a controlled ROS2 environment so the host need not install ROS2 locally.
- The code imports `standardbots` SDK — tests and changes involving the SDK should run inside the container or with that package installed.

## Editing & PR guidance for AI agents
- Small, focused edits only: preserve public APIs and examples in `README.md` unless the change is a clear correction.
- If you update runtime defaults (e.g. `ROS_DOMAIN_ID`), also update `README.md` and any shell scripts that assume the value.
- When adding scripts that interact with robots, include CLI examples in `README.md` that match the containerized run model (use `./run.sh python3 ...`).

## Quick examples to copy when making changes
- Detect bot id: reuse `detect_bot_id(node)` and the regex in `src/detect_bot_id.py`.
- Subscribe with matching QoS:
  - reliability: `ReliabilityPolicy.BEST_EFFORT`
  - history: `HistoryPolicy.KEEP_LAST`
  - depth: `10`
- Write poses flow: ensure SDK connection, unbrake, set ROS control to `Enabled`, do publishes, then restore state to `Disabled` on exit (see `src/write_poses.py`).

## What not to assume
- Do not assume the robot is on localhost; network adapter and Docker networking are required.
- Do not assume persistent ROS2 services — discovery is asynchronous. Use short spins and `detect_bot_id()` pattern.

## Where to look for more context
- Repository README: [README.md](README.md)
- Script examples: [src/detect_bot_id.py](src/detect_bot_id.py), [src/read_joint_states.py](src/read_joint_states.py), [src/write_poses.py](src/write_poses.py)

If any of these sections are unclear or you want more examples (unit test template, linting rules, or a small CI job), tell me which area to expand. 
