---
name: configure-renesas-debug
description: >
  Use this skill to create, update, or modify debug configurations for Renesas
  projects and Renesas devices in VS Code launch.json files.

  Proactively activate this skill when users:
  - Mention hardware debug, launch.json, or debug configuration
  - Ask to create, modify, or fix a hardware debug configuration setup
  - Refer to specific debuggers such as J-Link, E1, E2, E2LITE, E20, EZ, IECUBE, COMPORT, or Simulator
  - Request changes to target settings, hotplug, or server parameters

  This skill supports the following Renesas device families: Dialog, RA, RCAR,
  RH850, RL78, RX, RISCV, and RZ devices as well as the Simulator, Segger J-Link
  (SEGGERJLINKARM, SEGGERJLINKRX, SEGGERJLINKRISCV) and Renesas Emulators
  (E1, E2, E20, E2LITE, EZ, IECUBE, COMPORT).
---

# Renesas Hardware Launch Configuration Skill

This skill covers all tasks related to **hardware debug launch configuration**
in `.vscode/launch.json` for Renesas devices.

It acts as an **entry point** and routes the request to the correct
debugger-specific instructions.

## Skill Routing Logic

- If the request involves **SEGGER J-Link** → follow instructions in [SEGGER J-Link Hardware Debug Configuration Rules and Instructions](jlink.md)
- If the request involves **E2/E2Lite** → follow instructions in [E2/E2Lite Hardware Debug Configuration Rules and Instructions](e2-e2lite.md)
- If the request involves **Simulator** → follow instructions in [Simulator Debug Configuration Rules and Instructions](simulator.md)
- If the request involves **E1** → follow instructions in [E1 Hardware Debug Configuration Rules and Instructions](e1.md)
- If the request involves **E20** → follow instructions in [E20 Hardware Debug Configuration Rules and Instructions](e20.md)
- If the request involves **EZ** → follow instructions in [EZ Hardware Debug Configuration Rules and Instructions](ez.md)
- If the request involves **IECUBE** → follow instructions in [IECUBE Hardware Debug Configuration Rules and Instructions](iecube.md)
- If the request involves **COMPORT** → follow instructions in [COMPORT Hardware Debug Configuration Rules and Instructions](comport.md)

Debugger-specific files contain the detailed rules and constraints.