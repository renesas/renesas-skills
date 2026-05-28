# EZ Hardware Debug Configuration Rules and Instructions

This document defines **EZ Hardware Debug Configuration-specific rules, constraints, and templates** for Renesas hardware debug configurations in `.vscode/launch.json`.

This file is **not a standalone skill**. It provides **EZ Debug Configuration-specific constraints** for the **Renesas Hardware Launch Configuration** skill and must be applied **only when the selected debugger is EZ**.

## Scope

Apply the instructions in this file **only when all of the following are true**:

- The debugger type is **EZ**
- The debug adapter is Renesas hardware:
  - `"type": "renesas-hardware"`
  - `target.debuggerType` is set to `"EZ"`

## Core Assumptions

When this file is used:

- The user intent has already been classified as a **hardware debug launch configuration** request
- `.vscode/launch.json` is the target file
- Requests for non-EZ debuggers are assumed to have been filtered out by the entry skill

## EZ Debug Configuration Template

Use this template when **creating** a new EZ debug configuration:

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
        "debuggerType": "EZ"
      }
    }
  ],
  "version": "0.2.0"
}
```

### Template Notes

- The `device` and `deviceFamily` values shown above are **examples only**
- The `debuggerType` value must always be set to `"EZ"` and must not be changed
- Do **not** overwrite an existing configuration name unless the user explicitly requests it
- Do **not** guess placeholder values such as `DEVICE` or `DEVICE_FAMILY`
- The template above contains only required fields. Optional fields may be added when explicitly requested by the user — see [Additional Optional Fields](#additional-optional-fields)
- If any required values are unknown, ask the user to provide them before proceeding

## Supported Device Families

Supported device families depend on the EZ debugger and the selected device.

If the user requests an EZ configuration for a **non-supported family** (RX, RH850, RZ, RA, RISC-V MCU, ...):

- Do **not** create the configuration
- Inform the user that the EZ does not support that device family

## Required Fields

A valid **EZ** debug configuration **must include** the following fields
inside the `target` object:

- `device`
- `deviceFamily`
- `debuggerType`

### Required Value

- `debuggerType` **must** be `"EZ"`

### Strict Rule

If **any required field is missing**:

- **Do not guess**
- **Do not insert default values**
- **Always ask the user** to provide the missing field or fields before proceeding

## File Modification Rules

### General Rules

- Only modify `.vscode/launch.json` **when explicitly requested** by the user
- Never modify unrelated launch configurations
- Never apply these EZ-specific rules to other debuggers

### When Creating a New File or New Configuration

- Output a **complete, valid JSON file**
- Do **not** output JSON fragments or partial objects
- Ensure the result can be pasted directly into `.vscode/launch.json`
- If the `.vscode/` directory does not exist, note that it must be created before the file can be placed at `.vscode/launch.json`

### When Editing an Existing File

- Preserve all unrelated configurations
- Only modify the intended EZ-related configuration
- If multiple EZ debug configurations exist and the user does not specify which one to modify, ask the user to identify the configuration by `name` or position before proceeding

### JSON Validity Requirements

- Do not include trailing commas
- Ensure correct object nesting and structure
- The output must be valid JSON

## Target Object Modification Rules

For **EZ-specific parameter updates**:

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
        "device": "R7F122FGG",
        "deviceFamily": "RL78",
        "debuggerType": "EZ"
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
        "device": "R7F122FGG",
        "deviceFamily": "RL78",
        "debuggerType": "EZ"
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
  "device": "R7F122FGG",
  "deviceFamily": "RL78",
  "debuggerType": "EZ",
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

## Safety Rules

These rules apply in **all cases**:

- Never modify unrelated launch configurations
- Never apply EZ-specific rules to other debuggers
- Never guess required values
- Never silently insert default values
- Never use the EZ for unsupported device families

## Expected Behavior Summary

When handling EZ launch configuration requests:

1. Confirm the request is for:
   - `"type": "renesas-hardware"`
   - `target.debuggerType = "EZ"`
2. Confirm the target `deviceFamily` is supported by the EZ
3. Check that required fields exist:
   - `device`
   - `deviceFamily`
   - `debuggerType`
4. If required values are missing, ask the user before making changes
5. Modify only the intended EZ debug configuration
6. Keep unrelated configurations unchanged
7. Return valid JSON suitable for `.vscode/launch.json`
