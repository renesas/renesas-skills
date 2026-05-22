# Simulator Debug Configuration Rules and Instructions

This document defines **Simulator Debug Configuration-specific rules, constraints, and templates** for Renesas hardware debug configurations in `.vscode/launch.json`.

This file is **not a standalone skill**. It provides **Simulator-specific constraints** for the **Renesas Hardware Launch Configuration** skill and must be applied **only when the selected debugger is Simulator**.

## Scope

Apply the instructions in this file **only when all of the following are true**:

- The debugger type is **Simulator**
- The debug adapter is Renesas hardware:
  - `"type": "renesas-simulator"`
  - `target.debuggerType` is set to `"SIMULATOR"`

## Core Assumptions

When this file is used:

- The user intent has already been classified as a **hardware debug launch configuration** request
- `.vscode/launch.json` is the target file
- Requests for non-Simulator debuggers are assumed to have been filtered out by the entry skill

## Simulator Debug Configuration Template

Use this template when **creating** a new Simulator debug configuration:

```json
{
  "configurations": [
    {
      "type": "renesas-simulator",
      "request": "launch",
      "name": "Renesas GDB Simulator Debugging",
      "target": {
        "device": "DEVICE",
        "deviceFamily": "DEVICE_FAMILY",
        "debuggerType": "SIMULATOR"
      }
    }
  ],
  "version": "0.2.0"
}
```

### Template Notes

- The `device`, `deviceFamily` value shown above is an **example only**
- Do **not** overwrite an existing configuration name unless the user explicitly requests it
- Do **not** guess placeholder values such as `DEVICE` or `DEVICE_FAMILY`
- The template above contains only required fields. Optional fields may be added when explicitly requested by the user — see [Additional Optional Fields](#additional-optional-fields)
- If any required values are unknown, ask the user to provide them before proceeding

## Supported Device Families

The Simulator supports the following device families:

- `RX`
- `RL78`
- `RCAR`

If the user requests a Simulator configuration for a **non-supported family** (Dialog, RA, RH850, RISCV, RZ):

- Do **not** create the configuration
- Inform the user that the Simulator does not support that device family

## Required Fields

A valid **Simulator** debug configuration **must include** the following fields
inside the `target` object:

- `device`
- `deviceFamily`
- `debuggerType`

### Required Value

- `debuggerType` **must** be `"SIMULATOR"`

### Strict Rule

If **any required field is missing**:

- **Do not guess**
- **Do not insert default values**
- **Always ask the user** to provide the missing field or fields before proceeding

## File Modification Rules

### General Rules

- Only modify `.vscode/launch.json` **when explicitly requested** by the user
- Never modify unrelated launch configurations
- Never apply these Simulator-specific rules to other debuggers

### When Creating a New File or New Configuration

- Output a **complete, valid JSON file**
- Do **not** output JSON fragments or partial objects
- Ensure the result can be pasted directly into `.vscode/launch.json`

### When Editing an Existing File

- Preserve all unrelated configurations
- Only modify the intended Simulator-related configuration
- If multiple Simulator debug configurations exist and the user does not specify which one to modify, ask the user to identify the configuration by `name` or position before proceeding

### JSON Validity Requirements

- Do not include trailing commas
- Ensure correct object nesting and structure
- The output must be valid JSON

## Target Object Modification Rules

For **Simulator-specific parameter updates**:

- Only modify fields inside the `target` object unless the user explicitly requests changes outside it
- Do **not** change the following fields unless the user explicitly asks:
  - `type`
  - `request`
  - `name`

All other fields inside `target` that are **not mentioned by the user** must remain unchanged.

### Missing Target Object

If the selected configuration does **not** contain a `target` object:

- Do **not** create one automatically
- Ask the user whether the configuration should be replaced or updated explicitly

## Additional Optional Fields

The following fields are **not required** for a standard debug configuration but may be added when explicitly requested by the user.

### Fields at the same level as `target`

- `gdbArguments` — command line arguments passed to GDB during the debug session
- `initCommands` — list of GDB commands sent after attaching to target before loading image on target
- `preRunCommands` — list of GDB commands sent after loading image on target before resuming target
- `images` — defines the list of images to be downloaded during the debug session. Each item is an object with the following properties:
  - `file` *(required)* — path to the target file (e.g. `"${workspaceFolder}/build/image.elf"`)
  - `type` *(optional)* — type of file to be loaded, defaults to `"image-and-symbols"`. Valid values:
    - `"image-and-symbols"`
    - `"image-only"`
    - `"symbols"`
    - `"binary"`
  - `offset` *(optional)* — memory offset for the image, defaults to `"0x0"`
  - `program` *(optional)* — set `true` to declare this as the main program image

### Fields inside `target`

- `supportFolder` — path to the device family specific support folder. Defaults to the folder managed by the Renesas Debug extension

### Rules

- Do **not** add these fields unless the user explicitly requests them
- Do **not** guess or insert default values for these fields
- If the user requests one of these fields but does not provide a value, ask the user to provide the value before proceeding

### Example

```json
{
  "configurations": [
    {
      "type": "renesas-hardware",
      "request": "launch",
      "name": "Renesas GDB Hardware Debugging",
      "target": {
      "device": "R7FA4M2AD",
      "deviceFamily": "RA",
      "debuggerType": "SEGGERJLINKARM",
      "supportFolder": "${command:supportFolder}"
    },
      "gdbArguments": [],
      "preRunCommands": [],
      "initCommands": [],
      "images": [
        {
          "file": "${workspaceFolder}/build/image.elf",
          "type": "image-and-symbols",
          "program": true
        }
      ]
    }
  ],
  "version": "0.2.0"
}
```

## RTOS Support Rules (Simulator)

RTOS debugging with simulator is supported **only for RL78 device family**.  
RTOS debugging must **not** be enabled for other device families.  

When the user requests RTOS debugging for simulator:
- Verify that `target.deviceFamily` is `RL78`. If not, inform the user that RTOS debugging is not supported for that device family and do not proceed
- Add `"rtosIntegration": true` inside the `target` object (it must be at the same level as `device`, `deviceFamily`, and `debuggerType`)
- RTOS-related server parameters such as `"-uOSRestriction="` may be added **only when explicitly requested by the user**, and must be placed inside `target.serverParameters`

### Validation

- If `target` or `target.deviceFamily` is missing, ask the user to provide the missing field
- Do not guess or infer `deviceFamily`

### Decision flow

1. Check whether `target.debuggerType` is `SIMULATOR`
2. If yes, check whether `target.deviceFamily` is `RL78`
3. If `target.deviceFamily` is `RL78` and the user requests RTOS debugging, add `"rtosIntegration": true` inside `target`
4. Add `target.serverParameters["-uOSRestriction=": 1]` only if the user explicitly requests it

### Explicit request for `"-uOSRestriction="`

Explicit requests include:
- "enable OS restriction"
- "set `"-uOSRestriction="` to 1"
- "restrict thread checking to the current thread"

### Field Reference

Use this section to explain field behavior when the user asks.
- `"rtosIntegration": true`
  - Enables RTOS debugging
  - OS threads are shown in the Debug and Call Stack views
  - Thread context (stack frames, registers, variables) is available for each thread
- `"-uOSRestriction="`
  - A server parameter inside `target.serverParameters`
  - Controls how OS threads are checked in the Debug View
  - Helps improve debugger responsiveness when many OS threads exist

### Dependency Rule (RTOS)

- `"rtosIntegration"` enables RTOS debugging
- RTOS threads are available only when `"rtosIntegration"` is enabled
- `"-uOSRestriction="` controls OS thread checking behavior in the Debug View
  - `0` (default): do not restrict OS thread checking
  - `1`: restrict OS thread checking to the current thread
- `"-uOSRestriction="` is effective only when `"rtosIntegration": true` is enabled
- `"-uOSRestriction="` does **not** enable RTOS support by itself

### To disable RTOS support
- Either set `"rtosIntegration": false` inside the `target` object, or
- Remove the `"rtosIntegration"` field from the `target` object

### Example (enable RTOS)

```json
"target": {
  "device": "R7F100GSJ",
  "deviceFamily": "RL78",
  "debuggerType": "SIMULATOR",
  "rtosIntegration": true
}
```

### Example (enable RTOS with OS restriction enabled)

```json
"target": {
  "device": "R7F100GSJ",
  "deviceFamily": "RL78",
  "debuggerType": "SIMULATOR",
  "rtosIntegration": true,
  "serverParameters": {
    "-uOSRestriction=": 1
  }
}
```

### Example (disable RTOS — set the field to false):

```json
"target": {
  "device": "R7F100GSJ",
  "deviceFamily": "RL78",
  "debuggerType": "SIMULATOR",
  "rtosIntegration": false
}
```

### Example (disable RTOS — remove the field):

```json
"target": {
  "device": "R7F100GSJ",
  "deviceFamily": "RL78",
  "debuggerType": "SIMULATOR"
}
```

## Safety Rules

These rules apply in **all cases**:

- Never modify unrelated launch configurations
- Never apply Simulator-specific rules to other debuggers
- Never guess required values
- Never silently insert default values
- Never use the Simulator for unsupported device families

## Expected Behavior Summary

When handling Simulator launch configuration requests:

1. Confirm the request is for:
   - `"type": "renesas-simulator"`
   - `target.debuggerType = "SIMULATOR"`
2. Confirm the target `deviceFamily` is supported by the Simulator
3. Check that required fields exist:
   - `device`
   - `deviceFamily`
   - `debuggerType`
4. If required values are missing, ask the user before making changes
5. Modify only the intended Simulator debug configuration
6. Keep unrelated configurations unchanged
7. Return valid JSON suitable for `.vscode/launch.json`
