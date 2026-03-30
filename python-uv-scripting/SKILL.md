---
name: python-uv-scripting
description: Expert skill for creating portable, single-file Python scripts using the uv standard. Automates script initialization, dependency injection via uv add, and CLI scaffolding using Typer.
compatibility: Optimized for pi-coding-agent and uv (Astral).
metadata:
  version: "2.0"
  target_model: "qwen:3.5:27b"
---

# Python UV Scripting (v2)

Use this skill to build robust, self-contained Python tools in `~/uv_scripts/`. You must rely on `uv` CLI commands for structure and metadata management.

## Core Workflow

### Phase 1: Initialization
1.  **Create Directory**: Ensure `~/uv_scripts/` exists.
2.  **Initialize**: Run `uv init --script ~/uv_scripts/<filename>.py`.
    * *Note*: This automatically sets the `requires-python` version based on the system.
3.  **Inspect**: Read the newly created file to identify the `requires-python` value. This informs you which library versions are compatible.

### Phase 2: Dependency Injection
1.  **Add Libraries**: Use `uv add --script ~/uv_scripts/<filename>.py <package>`.
    * Always add `typer` and `rich` for CLI tools.
    * Monitor the command output for errors; if a dependency fails, resolve version conflicts before proceeding.

### Phase 3: Script Development
1.  **Edit Only**: Use the **EDIT** tool to modify the script body. Do not overwrite the file with a generic `write` command to preserve the `uv` metadata block.
2.  **Scaffolding**: If the script is a CLI, implement the `Typer` framework.

### Phase 4: Verification
1.  **Run/Test**: ONLY if the user explicitly asks to test or run the script.
2.  **Command**: Execute using `uv run ~/uv_scripts/<filename>.py` followed by any necessary arguments.

## Script Template (Internal Logic)
When editing the file, ensure the logic follows this structure:

```python
# /// script
# (Metadata managed by uv add)
# ///

import typer
from rich.console import Console

app = typer.Typer()
console = Console()

@app.command()
def main(task: str = typer.Argument(..., help="The task to perform")):
    """Description of what this script does."""
    console.print(f"Executing: {task}", style="bold blue")

if __name__ == "__main__":
    app()
```
## Best Practices
- Single File: Keep all logic, including helper functions, within the single .py file.
- Error Handling: Wrap main logic in try/except blocks using rich for pretty-printing errors.
- Type Hints: Use Python type hints for all function arguments to benefit Typer's CLI parsing.
