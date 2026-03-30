---
name: python-uv-scripting
description: Generates single-file Python scripts using the uv standard. Ensures dependencies are defined via inline metadata, uses the system's Python version, and utilizes the Typer framework for CLI tools.
compatibility: Requires uv (Astral) to be installed on the system.
metadata:
  version: "1.0"
  author: "Will Morris"
---

# Python UV Scripting Skill

Use this skill to generate portable, single-file Python scripts that manage their own dependencies using the `uv` script metadata format.

## Core Requirements

1.  **Environment Discovery**: Before writing the script, check the system Python version using `uv python find` or `python --version`.
2.  **Output Location**: All scripts MUST be saved in `~/uv_scripts/` if no specific path is provided by the user. This ensures a consistent location for all generated scripts.
3.  **Dependency Management**: Use the PEP 723 inline script metadata block (`# /// script`).
4.  **CLI Framework**: If the script requires command-line arguments or subcommands, you MUST use the **Typer** library.

## Workflow

### Step 1: Initialization
Ensure the target directory exists:
`mkdir -p ~/uv_scripts/`

### Step 2: Template Generation
Every script should follow this structural template:

```python
# /// script
# requires-python = ">=[SYSTEM_VERSION]"
# dependencies = [
#   "typer",
#   "rich",
# ]
# ///

import typer
from rich.console import Console

app = typer.Typer()
console = Console()

@app.command()
def main():
    """Default command description."""
    console.print("Hello from uv script!", style="bold green")

if __name__ == "__main__":
    app()
```

### Step 3: Execution
Inform the user they can run the script immediately without a virtual environment:
`uv run ~/uv_scripts/your-script.py`

## Best Practices
- Keep scripts to a single file.
- Always include `rich` for better terminal output if a CLI is requested.
- Use type hints for all function signatures.
- Provide clear help text for all CLI arguments using `Annotated` from the `typing` module.
