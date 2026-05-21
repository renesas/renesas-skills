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

## Device Family to Server Parameters Documentation Mapping

Each `deviceFamily` has a dedicated documentation file for supported server parameters.
When validating or explaining `serverParameters`, always reference the file that corresponds
to the `deviceFamily` of the current configuration — **do not use a generic or unrelated file**.

| deviceFamily | Server Parameters Documentation File |
|--------------|---------------------------------------|
| RL78         | server-parameters-rl78.rst            |
| RH850        | server-parameters-rh850.rst           |
| RX           | server-parameters-rx.rst              |
| RA           | server-parameters-ra.rst              |
| RISC-V MCU   | server-parameters-riscv.rst           |

### Rules

- Always determine `deviceFamily` **before** looking up any server parameter
- Use **only** the file mapped to the active `deviceFamily`
- **Never** cross-reference a parameter from a different family's file
- If `deviceFamily` is unknown or not yet confirmed, ask the user before proceeding

## E2/E2Lite Debug Configuration Template

Use this template when **creating** a new E2/E2Lite hardware debug configuration.

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

- `debuggerType` must be either `"E2"` or `"E2LITE"` depending on the emulator used
- `device` and `deviceFamily` values are **examples only**
- Do **not** overwrite an existing configuration name unless the user explicitly requests it
- Do **not** guess placeholder values such as `DEVICE` or `DEVICE_FAMILY`
- If any required values are unknown, ask the user to provide them before proceeding

> **The template above is the complete output for a creation request.**
> Do **not** add any field beyond what is shown here unless the user **explicitly** names
> that field in their request. Fields such as `images`, `gdbArguments`, `initCommands`,
> `preRunCommands`, and `supportFolder` are **never** added automatically —
> see [Additional Optional Fields](#additional-optional-fields).

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
- **Always ask the user** to provide the missing field(s) before proceeding

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

> **This rule applies to both create and edit operations.**
> When creating a new configuration, only include the fields the user asked for.
> Do not add extra fields inside or outside `target` because they seem useful or complete.

### Missing Target Object

If the selected configuration does **not** contain a `target` object:

- Do **not** create one automatically
- Ask the user whether the configuration should be replaced or updated explicitly

## Additional Optional Fields

The following fields are **not required** for a standard debug configuration but may be added when explicitly requested by the user.

> **Critical Rule**: These fields are **strictly opt-in**. Do **not** add any of them
> when creating or updating a configuration unless the user **explicitly** asks for that
> specific field by name. Do not add them as a convenience, as defaults, or because they
> appear in examples.

### Fields at the same level as `target`

- `gdbArguments` — command line arguments passed to GDB executable at startup (before GDB begins running)
- `initCommands` — list of GDB commands sent after attaching to target, before loading image on target
- `preRunCommands` — list of GDB commands sent after loading image on target, before resuming target
- `images` — defines the list of images to be downloaded during the debug session. Each item is an object with the following properties:
  - `file` *(required)* — path to the target file (e.g. `"${workspaceFolder}/build/image.elf"`)
  - `type` *(optional)* — type of file to be loaded, defaults to `"image-and-symbols"`. Valid values:
    - `"image-and-symbols"`
    - `"image-only"`
    - `"symbols"`
    - `"binary"`
  - `offset` *(optional)* — memory offset for the image, defaults to `"0x0"`
  - `program` *(optional)* — set `true` to declare this as the main program image

### GDB Command Reference for initCommands and preRunCommands

When the user asks for suggestions or help with commands to add to `initCommands` or
`preRunCommands`, follow these rules:

#### Command Source

All available GDB commands and their descriptions are documented in **`renesas-gdb-backends.rst`**.
Use this file as the **sole reference** when suggesting commands. Do not suggest commands
that are not listed in this file.

#### How to Suggest Commands

When the user describes what they want to achieve (e.g. "reset the target before running",
"set a breakpoint at hal_entry", "enable verbose output"):

1. Look up `renesas-gdb-backends.rst` for commands whose **Description** matches the user's intent
2. Check the **`Auto by Extension`** column for each matching command
3. Suggest the command with the appropriate notice based on the result:

**If `Auto by Extension = No`** → suggest freely:

> You can safely add this command to `initCommands` or `preRunCommands`.

**If `Auto by Extension = Yes`** → warn the user before suggesting:

> **`<command>`** is already sent automatically by the Renesas Debug extension.
> Adding it manually to `initCommands` or `preRunCommands` will cause it to be sent **twice**,
> which may lead to unexpected behavior or conflicts.
> Consider whether you still need to add it manually.

#### Auto Command Warning — Strict Rule

- **Never** silently suggest a command that has `Auto by Extension = Yes`
- **Always** show the warning above before including it in any suggested configuration
- If the user explicitly confirms they want to add it anyway, include it with an inline comment noting the duplicate risk

### Fields inside `target`

- `supportFolder` — path to the device family specific support folder. Defaults to the folder managed by the Renesas Debug extension

### Rules

- **Never** add any optional field unless the user explicitly names it in their request
- **Never** infer that the user wants an optional field based on context or common usage
- **Never** add optional fields as part of a "complete" or "best practice" configuration
- Do **not** guess or insert default values for these fields
- If the user requests one of these fields but does not provide a value, ask the user to provide the value before proceeding

**Minimal template — correct** (no optional fields added unless requested):

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
        "debuggerType": "E2"
      }
    }
  ],
  "version": "0.2.0"
}
```

**Wrong** — `images`, `gdbArguments`, etc. added without user request:

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
        "debuggerType": "E2"
      },
      "images": [...],
      "gdbArguments": []
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

## External Flash Debugging Rules (E2/E2Lite)

External Flash Debugging is supported **only for RA8 devices** (`target.device` starts with `R7KA8`).  
External Flash Debugging must **not** be enabled for other devices.

When the user requests External Flash Debugging support:
- Verify that device is an RA8 device (`target.device` starts with `R7KA8`). If not, inform the user that External Flash Debugging is not supported for that device and do not proceed
- Verify that `target.board` is present. If not, inform the user that `board` field is required for External Flash Debugging and request the user to add it manually
- Add `"externalFlashSupport": true` inside the `target` object

### Validation

- If `target` or `target.device` is missing, ask the user to provide the missing field
- Do not guess or infer `device`
- To verify: check that `target.device` starts with `R7KA8`
- If `target.board` is missing when enabling External Flash Debugging, do not proceed. Request the user to add `board` manually before enabling External Flash Debugging

### Dependency Rule (External Flash)

- `"externalFlashSupport": true` requires `target.board` to be present
- `target.board` is automatically added when the user creates the JSON file automatically
- If `target.board` is not present, the user must add it manually
- `target.board` is **not** required when External Flash Debugging is disabled

### Decision flow

1. Check whether `target.device` starts with `R7KA8` (RA8 devices)
2. If `target.device` is not an RA8 device, do not proceed
3. If yes, check whether `target.board` is present
4. If `target.board` is present and the user requests External Flash Debugging, add `"externalFlashSupport": true` inside `target`
5. If `target.board` is missing, request the user to add it manually before proceeding

### Field Reference

- `"externalFlashSupport": true`
  - Enables External Flash Debugging support
  - Allows the debugger to access and debug code running on external flash memory
- `target.board`
  - Identifies the board used for debugging
  - Required when External Flash Debugging is enabled
  - Automatically added when the JSON file is created automatically
  - Must be added manually if not present

### To disable External Flash Debugging support

- Either set `"externalFlashSupport": false` inside the `target` object, or
- Remove the `"externalFlashSupport"` field from the `target` object
- `target.board` is no longer required when External Flash Debugging is disabled

### Example (enable External Flash Debugging)

```json
"target": {
  "device": "R7KA8M2JF_CPU0",
  "deviceFamily": "RA",
  "debuggerType": "E2",
  "board": "EK-RA8M2",
  "externalFlashSupport": true
}
```

### Example (disable External Flash Debugging — set the field to false):

```json
"target": {
  "device": "R7KA8M2JF_CPU0",
  "deviceFamily": "RA",
  "debuggerType": "E2",
  "board": "EK-RA8M2",
  "externalFlashSupport": false
}
```

### Example (disable External Flash Debugging — remove the field):

```json
"target": {
  "device": "R7KA8M2JF_CPU0",
  "deviceFamily": "RA",
  "debuggerType": "E2"
}
```

## Server Parameters (E2/E2Lite Only)

### Placement

> **Critical Rule**: `serverParameters` **must always be placed inside the `target` object**.
> Placing it outside `target` is a structural error — never do this regardless of context.

`serverParameters` must be at the same level as `device`, `deviceFamily`, and `debuggerType` — all of which are inside `target`.

**Correct**

```json
{
  "type": "renesas-hardware",
  "request": "launch",
  "name": "Renesas GDB Hardware Debugging",
  "target": {
    "device": "R7FA4M2AD",
    "deviceFamily": "RA",
    "debuggerType": "E2",
    "serverParameters": {
      "-p": 64321
    }
  }
}
```

**Wrong — `serverParameters` outside `target`**

```json
{
  "type": "renesas-hardware",
  "request": "launch",
  "name": "Renesas GDB Hardware Debugging",
  "target": {
    "device": "R7FA4M2AD",
    "deviceFamily": "RA",
    "debuggerType": "E2"
  },
  "serverParameters": {
    "-p": 64321
  }
}
```

### Update Behavior

When adding or updating `serverParameters`:

- Preserve existing supported parameters unless the user explicitly requests replacement or removal
- Do not remove unrelated existing parameters

### Validation Rules (Strict)

When adding or updating a server parameter:

1. Determine the `deviceFamily` of the current configuration
2. Look up the corresponding documentation file using the **Device Family to Server Parameters Documentation Mapping** table
3. Locate the parameter entry in **that specific file**
4. Find the **`Emulator:`** field inside that parameter's entry
5. Check that `E2` or `E2Lite` is explicitly listed as one of the values in the `Emulator:` field — see [How to Read the Emulator Field](#how-to-read-the-emulator-field) for all valid E2/E2Lite name forms
6. If E2/E2Lite is confirmed, validate:
   - Correct data type: `string`, `number`, `boolean`, or `array`
   - Allowed values, if listed in the `Available Values:` field of that entry
7. Copy the parameter key **exactly** as it appears in the leftmost column of the entry — see [Server Parameter Key Name Rule](#server-parameter-key-name-rule)

### How to Read the Emulator Field

Each parameter entry in the `.rst` file contains an `Emulator:` field that lists which
debuggers support it. The field may use either the short name or the full debuggerType name
for E2/E2Lite. Both forms are equivalent and must be treated as E2/E2Lite support.

**E2/E2Lite is considered supported if the `Emulator:` field contains any of the following:**

| Form | Example in Emulator field |
|------|---------------------------|
| Short name (E2) | `E2` |
| Short name (E2Lite) | `E2Lite` |
| Full debuggerType (E2) | `E2` |
| Full debuggerType (E2Lite) | `E2LITE` |

When checking support, match against **all forms** — `E2`, `E2Lite`, and `E2LITE`.

Example entries and their interpretation:

```
| Emulator: E2, E2Lite, J-Link |   → supported for E2 and E2Lite
| Emulator: E2 |                   → supported for E2 only, not E2Lite
| Emulator: E2Lite |               → supported for E2Lite only, not E2
| Emulator: J-Link |               → NOT supported for E2/E2Lite
```

- **Matching E2/E2Lite variant present** → parameter is supported, proceed with validation
- **No E2/E2Lite variant present** → parameter is not supported, do not add it
- **`Emulator:` field missing entirely** → treat as unsupported, do not guess

If the parameter entry cannot be found in the documentation file:

- Do **not** add or modify the parameter
- Ask the user to provide the official parameter reference or allow a documentation lookup step

### Unsupported Parameters

If the `Emulator:` field does **not** include `E2` or `E2Lite` (matching the active debuggerType), respond with:

> `"<option>" is not supported for E2/E2Lite and will not be added.`

Then:

- Do **not** add the parameter
- Do **not** assume compatibility based on other emulators listed
- Do **not** guess support if the `Emulator:` field is missing or ambiguous

### Server Parameter Key Name Rule

> **Critical Rule**: The parameter key must be copied **exactly** as it appears in the
> documentation file — character by character, including punctuation, spacing, and trailing
> `=` if present. **Never rewrite, normalize, or guess the key name.**

Each parameter entry in the `.rst` file has the exact key in the leftmost column of the table.
Use that value verbatim as the JSON key in `serverParameters`.

**Correct** — key copied exactly from documentation:

```json
"serverParameters": {
  "-uInteface=": "SWD"
}
```

**Wrong** — key rewritten or normalized by AI:

```json
"serverParameters": {
  "-interface": "SWD",
  "-uInterface": "SWD",
  "-uInterface=": "SWD"
}
```

If the user describes a parameter by intent (e.g. "set interface to SWD") rather than by exact key:

1. Look up the documentation file for the active `deviceFamily`
2. Find the entry whose `Description:` matches the user's intent
3. Copy the exact key from the leftmost column of that entry
4. Use that exact key — do not derive or guess it from the description

## Explaining Server Parameters

When the user asks for the **meaning**, **purpose**, or **explanation**
of any server parameter, follow these rules strictly.

### Do

- Determine the `deviceFamily` of the current configuration
- Look up the correct documentation file using the **Device Family to Server Parameters Documentation Mapping** table
- Locate the parameter entry in that file and check its **`Emulator:`** field
- Only explain the parameter if `E2` or `E2Lite` (matching the active `debuggerType`) is listed in the `Emulator:` field
- When explaining, include all available fields from the entry:
  - `Description:`
  - `Default Values:`
  - `Emulator:` — list which debuggers support it
  - `Available Values:`
  - `Example:`
- Clearly indicate when text is drawn directly from official documentation

### Do Not

- Guess meaning or behavior not documented in the entry
- Infer undocumented behavior from similar parameters
- Explain a parameter if `E2`/`E2Lite` is not listed in its `Emulator:` field
- Present undocumented assumptions as facts

### If the parameter is not in the documentation

Clearly state:

- *`"<option>"` is not documented in the official documentation for this device family.*

### If the parameter exists but does not support E2/E2Lite

Clearly state:

- *`"<option>"` exists in the documentation but is not supported for E2/E2Lite (Emulator field does not include E2 or E2Lite).*

If documentation cannot be verified, state that clearly and do not provide an interpretation.

## Safety Rules

These rules apply in **all cases**:

- Never modify unrelated launch configurations
- Never apply E2/E2Lite-specific rules to other debuggers
- Never guess required values
- Never silently insert default values
- Never assume a server parameter is supported without documentation verification
- Never look up server parameters from a documentation file that does not match the active `deviceFamily`
- **Never place `serverParameters` outside the `target` object** — it must always be nested inside `target`
- **Never add optional fields** (`images`, `gdbArguments`, `initCommands`, `preRunCommands`, `supportFolder`) unless the user explicitly names them in their request
- **Never rewrite or normalize a server parameter key name** — always copy it verbatim from the leftmost column of the documentation table

## Expected Behavior Summary

When handling E2/E2Lite launch configuration requests:

1. Confirm the request is for:
   - `"type": "renesas-hardware"`
   - `target.debuggerType` is `"E2"` or `"E2LITE"`

2. Check that required fields exist:
   - `device`
   - `deviceFamily`
   - `debuggerType`

3. If required values are missing, ask the user before making changes

4. Modify only the intended E2/E2Lite debug configuration

5. Keep unrelated configurations unchanged

6. **Output only the fields the user asked for** — do not add optional fields (`images`, `gdbArguments`, `initCommands`, `preRunCommands`, `supportFolder`) unless explicitly requested by name

7. When adding `serverParameters`:
   - Validate against the correct documentation file for the active `deviceFamily` using the **Device Family to Server Parameters Documentation Mapping** table
   - Confirm `E2` or `E2Lite` is listed in the `Emulator:` field of the parameter entry
   - Copy the parameter key **verbatim** from the leftmost column of the documentation table — do not rewrite or normalize it
   - Place `serverParameters` inside `target` — never outside

8. Return valid JSON suitable for `.vscode/launch.json`
