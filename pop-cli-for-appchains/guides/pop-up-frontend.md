---
description: Start a frontend dev server with Pop CLI.
---

# Start a frontend dev server

Use `pop up frontend` to run a frontend dev server for your project.

## Prerequisites

- A frontend project directory.
- Pop CLI installed.

## Usage

```shell
pop up frontend [--path PATH]
```

## Options

| Option | Description |
| --- | --- |
| `--path <PATH>` | Frontend directory. Defaults to `./frontend` or the current directory if it looks like a frontend project. |

## Behavior

- If the path is missing, Pop CLI prompts you to select a directory.
- Pop CLI installs dependencies and runs the dev server using the detected package manager.

## Example

```shell
pop up frontend --path ./frontend
```
