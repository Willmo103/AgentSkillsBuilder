# CLI Standards for uv_scripts

When creating a CLI tool for the user, follow these Typer conventions:

## Argument Handling

- Use `Annotated` for command arguments to provide help text.
- Example: `def main(name: Annotated[str, typer.Argument(help="The name to greet")]):`

## Error Handling

- Use `typer.Exit(code=1)` for clean exits on failure.
- Wrap complex logic in `try/except` blocks that use `rich` to print error messages.

## Versioning

- If requested, add a `--version` callback using `typer.Option`.
