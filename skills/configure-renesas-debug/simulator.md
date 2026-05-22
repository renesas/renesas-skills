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

The following fields are **not required** for a standard debug configuration but may be added when the user asks for them.

> **Critical Rule**: These fields are **strictly opt-in**. Do **not** add any of them
> when creating or updating a configuration unless the user **explicitly** asks for that
> specific field by name. Do not add them as a convenience, as defaults, or because they
> appear in examples.

### Fields at the same level as `target`

- `gdbArguments`
- `initCommands`
- `preRunCommands`
- `images`

#### `gdbArguments`

Command line arguments passed to GDB during debug session.

```json
"gdbArguments": ["<argument1>", "<argument2>"]
```

##### Explicit Requests

Explicit requests include phrases such as:
- "add gdbArguments"
- "add arguments to GDB"
- "pass arguments to GDB"

##### Adding `gdbArguments`

When the user requests `gdbArguments`, follow these steps:

1. Ask the user for the argument value:
   > What argument do you want to pass to GDB? (e.g. `"--nx"`, `"--quiet"`)

2. Add the argument to the `gdbArguments` array.

3. Ask the user if they want to add another argument:
   > Do you want to add another argument?
   - If yes, repeat steps 1–3
   - If no, finalize the `gdbArguments` array

##### Updating or Removing `gdbArguments`

- To **add** an argument to an existing list: append to the existing array without removing other entries.
- To **remove** a specific argument: delete only that entry from the array.
- To **remove the entire field**: delete the `gdbArguments` key from the configuration.

##### Example

```json
"gdbArguments": ["--nx", "--quiet"]
```

#### `initCommands` and `preRunCommands`

- `initCommands` — list of GDB commands sent after attaching to target, before loading image on target
- `preRunCommands` — list of GDB commands sent after loading image on target, before resuming target

##### GDB Command Reference for `initCommands` and `preRunCommands`

###### Command Source

All available GDB commands are listed in **`renesas-gdb-backends.rst`**.
Use this file as the **only reference** when suggesting commands. Do not suggest commands that are not listed in this file.

If `renesas-gdb-backends.rst` is not available or the requested command is not found, do not suggest any command. Inform the user:
> Command suggestions are not available at this time. Please try again later or contact support.

Do **not** suggest or add any command that is absent from `renesas-gdb-backends.rst`.

###### Field Placement Guide

When suggesting a command, always inform the user which field is the right place:
- Use `initCommands` for commands that should run **after connecting to target**, before loading image (e.g. setup, reset)
- Use `preRunCommands` for commands that should run **after loading image**, before running target (e.g. set breakpoint, configure memory)

Based on the user's intent, suggest which field is the best place for the command.

Example:
```json
"initCommands": ["<command>"]
// or
"preRunCommands": ["<command>"]
```

###### How to Suggest Commands

When the user describes what they want to do (e.g. "reset the target before running", "set a breakpoint at hal_entry", "enable verbose output"):

1. Look up `renesas-gdb-backends.rst` for commands that match the user's intent
2. Check the **`Auto by Extension`** column for each matching command
3. Suggest the command based on the result:

**If `Auto by Extension = No`** → suggest freely using the Field Placement Guide above.

**If `Auto by Extension = Yes`** → warn the user first:
> **`<command>`** is already sent automatically by the Renesas Debug Extension.
> Adding it manually will cause it to be sent **twice**, which may lead to unexpected behavior.
> Consider whether you still need to add it manually.

If the user confirms they still want to add it, follow the Field Placement Guide above and inform the user:
> **Note**: This command is already sent automatically by the extension and may cause duplicate behavior.

```json
"initCommands": ["<command>"]
// or
"preRunCommands": ["<command>"]
```

###### Auto Command Warning — Strict Rule

- **Never** suggest a command with `Auto by Extension = Yes` without showing the warning above
- **Always** show the warning before adding it to any suggested configuration

##### Adding `initCommands` or `preRunCommands`

When the user requests `initCommands` or `preRunCommands`, follow these steps:

1. Based on the user's intent, look up the matching command in `renesas-gdb-backends.rst` and determine the correct field using the Field Placement Guide above.

2. Ask the user to confirm the command to add:
   > I'll add `<command>` to `<field>`. Does that look correct?

3. Add the command(s) to the appropriate field.

4. Ask the user if they want to add another command:
   > Do you want to add another command?
   - If yes, repeat steps 1–4
   - If no, finalize the array

##### Updating or Removing `initCommands` / `preRunCommands`

- To **add** a command to an existing list: append to the existing array without removing other entries.
- To **remove** a specific command: delete only that entry from the array.
- To **remove the entire field**: delete the field key from the configuration.

##### Examples

**`preRunCommands`** example:

```json
{
  "configurations": [
    {
      "type": "renesas-hardware",
      "request": "launch",
      "name": "Renesas GDB Hardware Debugging",
      "target": {
        "device": "R5F565NE",
        "deviceFamily": "RX",
        "debuggerType": "SIMULATOR"
      },
      "preRunCommands": ["monitor reset"]
    }
  ],
  "version": "0.2.0"
}
```

**`initCommands`** example:

```json
{
  "configurations": [
    {
      "type": "renesas-hardware",
      "request": "launch",
      "name": "Renesas GDB Hardware Debugging",
      "target": {
        "device": "R5F565NE",
        "deviceFamily": "RX",
        "debuggerType": "SIMULATOR"
      },
      "initCommands": ["monitor reset"]
    }
  ],
  "version": "0.2.0"
}
```

#### `images`

List of images to download during the debug session. Each item has the following properties:
- `file` *(required)* — path to the image file (e.g. `"${workspaceFolder}/build/image.elf"`)
- `type` *(optional)* — type of file to load, defaults to `"image-and-symbols"`. Valid values:
  - `"image-and-symbols"`
  - `"image-only"`
  - `"symbols"`
  - `"binary"`
- `offset` *(optional)* — memory offset for the image, defaults to `"0x0"`
- `program` *(optional)* — set `true` to declare the main program image, defaults to `true`

If not defined, defaults to:
```json
"images": [
  {
    "file": "${command:program}",
    "type": "image-and-symbols",
    "program": true
  }
]
```

##### Explicit Requests

Explicit requests include phrases such as:
- "add images"
- "add images field"
- "configure images"
- "download multiple images during debugging"
- "load .elf file during debug session"

##### Adding `images`

When the user requests `images`, follow these steps for each image entry:

1. Ask the user for `file` (required):
   > What is the path to the image file? (e.g. `"${workspaceFolder}/build/image.elf"`)

2. Ask the user for `type` (optional):
   > What type of file do you want to load? Valid values: `"image-and-symbols"`, `"image-only"`, `"symbols"`, `"binary"`. Leave blank to skip (default: `"image-and-symbols"`).

3. Ask the user for `offset` (optional):
   > What is the memory offset for the image? Leave blank to skip (default: `"0x0"`).

4. Ask the user for `program` (optional). **Only ask this if the user is adding more than one image**, since `program: true` is only meaningful to distinguish the primary image from secondary ones:
   > Is this the main program image? (`true` or `false`). Leave blank to skip (default: `true`).

5. Add the item to the `images` array. Always include `file`. For optional fields (`type`, `offset`, `program`), **only include them if the user provided a non-blank value** — do not add fields the user left blank.

6. Ask the user if they want to add another image:
   > Do you want to add another image?
   - If yes, repeat steps 1–6
   - If no, finalize the `images` array

##### Updating or Removing `images`

- To **add** an image to an existing list: append the new entry to the existing array without modifying other entries.
- To **update** a property of an existing image: edit only the specified property of the matching entry (identified by `file` path).
- To **remove** a specific image: delete only that entry from the array.
- To **remove the entire field**: delete the `images` key from the configuration.

##### Example

```json
"images": [
  {
    "file": "${workspaceFolder}/build/image.elf",
    "type": "image-and-symbols",
    "program": true
  },
  {
    "file": "${workspaceFolder}/build/image2.elf",
    "type": "image-only"
  }
]
```

### Fields inside `target`

- `supportFolder`

#### `supportFolder`

Path to the device family specific support folder. Defaults to the Renesas Debug Extension managed support folder.

##### Explicit Requests

Explicit requests include phrases such as:
- "add supportFolder"
- "set support folder"
- "specify support folder path"

##### Adding `supportFolder`

When the user requests `supportFolder`, follow these steps:

1. Ask the user for the path:
   > What is the path to the support folder? (e.g. `"${command:supportFolder}"`)

2. Add `supportFolder` inside the `target` object based on the user's answer.

##### Updating or Removing `supportFolder`

- To **update**: replace the existing value with the new path provided by the user.
- To **remove**: delete the `supportFolder` key from inside the `target` object.

##### Example

```json
"target": {
  "device": "R5F565NE",
  "deviceFamily": "RX",
  "debuggerType": "SIMULATOR",
  "supportFolder": "${command:supportFolder}"
}
```

### Rules

- **Never** add any optional field unless the user explicitly asks for it by name. Do not infer intent from context.
- If the user asks for a field but does not give a value, ask for the value before proceeding.
- For `images`, only `file` is required. Do not include `type`, `offset`, or `program` in the output unless the user provides a non-blank value for them during the guided flow.
- For `initCommands` and `preRunCommands`, only suggest commands listed in `renesas-gdb-backends.rst`. 
  If `renesas-gdb-backends.rst` is not available or the requested command is not found, 
  do not suggest any command. Inform the user:
  > Command suggestions are not available at this time. Please try again later or contact support.
- When updating any field, only modify the specific entries or values the user asked to change. Do not touch unrelated fields.
- When removing any field, delete only the field the user asked to remove. Do not modify surrounding fields.

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
