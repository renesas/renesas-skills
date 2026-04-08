# SEGGER J-Link Hardware Debug Configuration Rules and Inst

This document defines **SEGGER J-Link hardware debug configuration–specific rules, constraints, and templates** for Renesas hardware debug configurations in `.vscode/launch.json`.

This file is **not a standalone skill**. It provides **J-Link Debug Configuration-specific constraints** for the **Renesas Hardware Launch Configuration** skill and must be applied **only when the selected debugger is SEGGER J-Link**.

## Scope

Apply the instructions in this file **only when all of the following are true**:

- The debugger type is **SEGGER J-Link**
- The launch configuration uses Renesas hardware debugging:
  - `"type": "renesas-hardware"`
  - `target.debuggerType` is one of:
    - `"SEGGERJLINKARM"`
    - `"SEGGERJLINKRX"`
    - `"SEGGERJLINKRISCV"`

## Core Assumptions

When this file is used:

- The user intent has already been classified as a **hardware debug launch configuration** request
- `.vscode/launch.json` is the target file
- Requests for non-J-Link debuggers are assumed to have been filtered out by the entry skill

## Supported Device Families

The **SEGGER J-Link** supports the following device families:

- `RX`
- `RA`
- `RISC-V MCU` (canonical deviceFamily value for RISCV targets)
- `RZ`
- `Dialog`

If the user requests a **SEGGER J-Link** configuration for a **non-supported family** (e.g. `RL78`, `RH850`):

- Do **not** create the configuration
- Inform the user that **SEGGER J-Link does not support that device family**

## J-Link Device Family and debuggerType Mapping

The value of `target.debuggerType` **must match** the architecture of the selected
`deviceFamily`.

| deviceFamily | debuggerType     |
|--------------|------------------|
| RA           | SEGGERJLINKARM   |
| RZ           | SEGGERJLINKARM   |
| Dialog       | SEGGERJLINKARM   |
| RX           | SEGGERJLINKRX    |
| RISC-V MCU   | SEGGERJLINKRISCV |

## J-Link Debug Configuration Template

Use this template when **creating** a new J-Link hardware debug configuration.

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
        "debuggerType": "DEBUGGER_TYPE"
      }
    }
  ],
  "version": "0.2.0"
}
```

### Template Notes

- `DEBUGGER_TYPE` must be one of:
  - `"SEGGERJLINKARM"`
  - `"SEGGERJLINKRX"`
  - `"SEGGERJLINKRISCV"`
- `device` and `deviceFamily` values are **examples only**
- Do **not** overwrite an existing configuration name unless the user explicitly requests it
- Do **not** guess placeholder values

## Required Fields

A valid **J-Link** debug configuration **must include** the following fields
inside the `target` object:

- `device`
- `deviceFamily`
- `debuggerType`

### Required Values

- `debuggerType` **must** be one of:
  - `"SEGGERJLINKARM"`
  - `"SEGGERJLINKRX"`
  - `"SEGGERJLINKRISCV"`
- The value **must match** the selected `deviceFamily` according to the mapping rules

### Strict Rule

If **any required field is missing**:

- **Do not guess**
- **Do not insert default values**
- **Always ask the user** to provide the missing field(s) before proceeding


## File Modification Rules

### General Rules

- Only modify `.vscode/launch.json` **when explicitly requested** by the user
- Never modify unrelated launch configurations
- Never apply these J-Link-specific rules to other debuggers

### When Creating a New File or New Configuration

- Output a **complete, valid JSON file**
- Do **not** output JSON fragments or partial objects
- Ensure the result can be pasted directly into `.vscode/launch.json`

### When Editing an Existing File

- Preserve all unrelated configurations
- Only modify the intended J-Link-related configuration
- If multiple J-Link debug configurations exist and the user does not specify which one to modify, ask the user to identify the configuration by `name` or position before proceeding

### JSON Validity Requirements

- Do not include trailing commas
- Ensure correct object nesting and structure
- The output must be valid JSON

## Target Object Modification Rules

For **J-Link-specific parameter updates**:

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

## Hotplug Support Rules (J-Link)

When the user requests hotplug support for J-Link:

- Add `"hotPlug": true` inside the `target` object

**To disable hotplug support:**

- Either set `"hotPlug": false` inside the `target` object, or
- Remove the `"hotPlug"` field from the `target` object

Example (enable hotplug):

```json
"target": {
  "device": "R7FA4M2AD",
  "deviceFamily": "RA",
  "debuggerType": "SEGGERJLINKARM",
  "hotPlug": true
}
```

Example (disable hotplug — set the field to false):

```json
"target": {
  "device": "R7FA4M2AD",
  "deviceFamily": "RA",
  "debuggerType": "SEGGERJLINKARM",
  "hotPlug": false
}
```

Example (disable hotplug — remove the field):

```json
"target": {
  "device": "R7FA4M2AD",
  "deviceFamily": "RA",
  "debuggerType": "SEGGERJLINKARM"
}
```

## Server Parameters (J-Link Only)

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
- Verify that the relevant documentation indicates support for **J-Link**
- Validate:
  - Correct data type: `string`, `number`, `boolean`, or `array`
  - Allowed values, if specified in the documentation

If official documentation cannot be verified:

- Do **not** add or modify the parameter
- Ask the user to provide the official parameter reference or allow a documentation lookup step

### Unsupported Parameters

If the requested parameter is **not supported for J-Link**, respond with:

> `"<option>" is not supported for J-Link and will not be added.`

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

#### Interface (If Supported)

```json
"serverParameters": {
  "-uInteface=": "SWD"
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
- Never apply J-Link-specific rules to other debuggers
- Never guess required values
- Never silently insert default values
- Never assume a server parameter is supported without documentation verification

## Expected Behavior Summary

When handling J-Link launch configuration requests:

1. Confirm the request is for:
   - `"type": "renesas-hardware"`
   - Confirm that target.debuggerType matches the selected deviceFamily according to the J-Link mapping rules

2. Check that required fields exist:
   - `device`
   - `deviceFamily`
   - `debuggerType`
3. If required values are missing, ask the user before making changes
4. Modify only the intended J-Link debug configuration
5. Keep unrelated configurations unchanged
6. Validate `serverParameters` against official documentation `server-parameters-ra.rst` before adding or changing them
7. Return valid JSON suitable for `.vscode/launch.json`
