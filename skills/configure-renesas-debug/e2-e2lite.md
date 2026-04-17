# E2/E2Lite Hardware Debug Configuration Rules and Instructions

This document defines **E2/E2Lite Hardware Debug Configuration-specific rules, constraints, and templates** for Renesas hardware debug configurations in `.vscode/launch.json`.

This file is **not a standalone skill**. It provides **E2/E2Lite Debug Configuration-specific constraints** for the **Renesas Hardware Launch Configuration** skill and must be applied **only when the selected debugger is Renesas E2 or E2Lite (E2, E2LITE)**.

## Scope

Apply the instructions in this file **only when all of the following are true**:

- The debugger type is **Renesas E2 or E2Lite**
- The debug adapter is Renesas hardware:
  - `"type": "renesas-hardware"`
  - `target.debuggerType` is set to `"E2"` or `"E2LITE"`

## Core Assumptions

When this file is used:

- The user intent has already been classified as a **hardware debug launch configuration** request
- `.vscode/launch.json` is the target file
- Requests for non-E2/E2Lite debuggers are assumed to have been filtered out by the entry skill

## Supported Device Families

The **E2/E2Lite** supports the following device families:

- `RL78`
- `RH850`
- `RX`
- `RA`
- `RISC-V MCU` (canonical deviceFamily value for RISCV targets)

If the user requests a **E2/E2Lite** configuration for a **non-supported family** (e.g. `Dialog`, `RZ`):

- Do **not** create the configuration
- Inform the user that **E2/E2Lite does not support that device family**

## E2/E2Lite Debug Configuration Template

Use this template when **creating** a new E2/E2Lite hardware debug configuration:

```json
{
  "configurations": [
    {
      "type": "renesas-hardware",
      "request": "launch",
      "name": "Renesas GDB Hardware Debugging",
      "target": {
        "device": "DEVICE",
        "deviceFamily": "DEVICE_FAMILY",
        "debuggerType": "E2"
      }
    }
  ],
  "version": "0.2.0"
}
```

### Template Notes

- The `device`, `deviceFamily` value shown above is an **example only**
- `debuggerType` can be either `"E2"` or `"E2LITE"` depending on the emulator used
- Do **not** overwrite an existing configuration name unless the user explicitly requests it
- Do **not** guess placeholder values such as `DEVICE` or `DEVICE_FAMILY`
- The template above contains only required fields. Optional fields may be added when explicitly requested by the user — see [Additional Optional Fields](#additional-optional-fields)
- If any required values are unknown, ask the user to provide them before proceeding

## Required Fields

A valid **E2/E2Lite** debug configuration **must include** the following fields
inside the `target` object:

- `device`
- `deviceFamily`
- `debuggerType`

### Required Value

- `debuggerType` **must** be either `"E2"` or `"E2LITE"`

### Strict Rule

If **any required field is missing**:

- **Do not guess**
- **Do not insert default values**
- **Always ask the user** to provide the missing field or fields before proceeding

## File Modification Rules

### General Rules

- Only modify `.vscode/launch.json` **when explicitly requested** by the user
- Never modify unrelated launch configurations
- Never apply these E2/E2Lite-specific rules to other debuggers

### When Creating a New File or New Configuration

- Output a **complete, valid JSON file**
- Do **not** output JSON fragments or partial objects
- Ensure the result can be pasted directly into `.vscode/launch.json`

### When Editing an Existing File

- Preserve all unrelated configurations
- Only modify the intended E2/E2Lite-related configuration
- If multiple E2/E2Lite debug configurations exist and the user does not specify which one to modify, ask the user to identify the configuration by `name` or position before proceeding

### JSON Validity Requirements

- Do not include trailing commas
- Ensure correct object nesting and structure
- The output must be valid JSON

## Target Object Modification Rules

For **E2/E2Lite-specific parameter updates**:

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

## Hotplug Support Rules (E2/E2Lite)

When the user requests hotplug support for E2/E2Lite:

- Add `"hotPlug": true` inside the `target` object

**To disable hotplug support:**

- Either set `"hotPlug": false` inside the `target` object, or
- Remove the `"hotPlug"` field from the `target` object

Example (enable hotplug):

```json
"target": {
  "device": "R7FA4M2AD",
  "deviceFamily": "RA",
  "debuggerType": "E2",
  "hotPlug": true
}
```

Example (disable hotplug — set the field to false):

```json
"target": {
  "device": "R7FA4M2AD",
  "deviceFamily": "RA",
  "debuggerType": "E2",
  "hotPlug": false
}
```

Example (disable hotplug — remove the field):

```json
"target": {
  "device": "R7FA4M2AD",
  "deviceFamily": "RA",
  "debuggerType": "E2"
}
```

## Server Parameters (E2/E2Lite Only)

### Placement

- `serverParameters` **must** be placed inside the `target` object
- It must be at the same level as:
  - `device`
  - `deviceFamily`
  - `debuggerType`
  - `hotPlug` if present

### Update Behavior

When adding or updating `serverParameters`:

- Preserve existing supported parameters unless the user explicitly requests replacement or removal
- Do not remove unrelated existing parameters

### Validation Rules (Strict)

When adding or updating a server parameter:

- Verify that the option exists in the **official Renesas documentation, primarily [GDB Server Parameters](server-parameters-ra.rst)**
- Verify that the relevant documentation indicates support for **E2/E2Lite**
- Validate:
  - Correct data type: `string`, `number`, `boolean`, or `array`
  - Allowed values, if specified in the documentation

If official documentation cannot be verified:

- Do **not** add or modify the parameter
- Ask the user to provide the official parameter reference or allow a documentation lookup step

### Unsupported Parameters

If the requested parameter is **not supported for E2/E2Lite**, respond with:

> `"<option>" is not supported for E2/E2Lite and will not be added.`

Then:

- Do **not** add the parameter
- Do **not** assume compatibility
- Do **not** guess support if unsure

### Server Parameter Examples

#### Port Number

```json
"serverParameters": {
  "-p": 64321
}
```

## Explaining Server Parameters

When the user asks for the **meaning**, **purpose**, or **explanation**
of any server parameter, follow these rules strictly.

### Do

- Use **official documentation text only** → follow in [GDB Server Parameters](server-parameters-ra.rst)
- Include, if available:
  - Description
  - Allowed values
  - Examples
- Clearly indicate when text is quoted directly from official documentation

### Do Not

- Guess
- Infer undocumented behavior
- Present undocumented assumptions as facts

### If the Parameter Is Not Documented

Clearly state:

- *It is not documented in the official documentation.*

If documentation cannot be verified, state that clearly and do not provide an interpretation.

## Safety Rules

These rules apply in **all cases**:

- Never modify unrelated launch configurations
- Never apply E2/E2Lite-specific rules to other debuggers
- Never guess required values
- Never silently insert default values
- Never assume a server parameter is supported without documentation verification

## Expected Behavior Summary

When handling E2/E2Lite launch configuration requests:

1. Confirm the request is for:
   - `"type": "renesas-hardware"`
   - `target.debuggerType = "E2"` or `target.debuggerType = "E2LITE"`
2. Check that required fields exist:
   - `device`
   - `deviceFamily`
   - `debuggerType`
3. If required values are missing, ask the user before making changes
4. Modify only the intended E2/E2Lite debug configuration
5. Keep unrelated configurations unchanged
6. Validate `serverParameters` against official documentation [GDB Server Parameters](server-parameters-ra.rst) before adding or changing them
7. Return valid JSON suitable for `.vscode/launch.json`