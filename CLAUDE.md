# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project state

This is a freshly scaffolded `uv init --package` project and is still essentially empty: the only
source file is `src/claude_code_master_class/__init__.py`, which contains a single `main()` that
prints a greeting. `README.md` is empty, there are no tests, no lint config, and no `uv.lock`.

Treat the structure below as conventions to follow as code gets added, not as a description of
existing architecture.

## Toolchain

- Package/environment manager: **uv** (0.12.6). Build backend is `uv_build`, so the project is
  installed from `src/` as a real package — not run as loose scripts.
- Python: **>= 3.14**, pinned to 3.14 in `.python-version`. Anything added must work on 3.14;
  `uv` will fetch that interpreter automatically.
- `dependencies` in `pyproject.toml` is empty. Add dependencies with `uv add <pkg>` (which edits
  `pyproject.toml` and updates `uv.lock`) rather than editing the file by hand or using pip.

## Commands

```powershell
uv sync                                  # create/refresh .venv from pyproject + lock
uv run claude-code-master-class          # run the console-script entry point
uv run python -c "..."                   # run anything inside the project env
uv add <pkg>                             # add a runtime dependency
uv add --dev <pkg>                       # add a dev-only dependency
uv build                                 # build sdist + wheel via uv_build
```

No test runner, formatter, or linter is configured yet. If tests are needed, add pytest as a dev
dependency (`uv add --dev pytest`) and run:

```powershell
uv run pytest                            # whole suite
uv run pytest tests/test_foo.py          # one file
uv run pytest tests/test_foo.py::test_bar # one test
uv run pytest -k "substring"             # by name match
```

## Layout conventions

- Package code lives under `src/claude_code_master_class/`. The `src/` layout means imports resolve
  through the installed package, so run code via `uv run` rather than plain `python file.py`.
- The CLI surface is declared in `[project.scripts]` as
  `claude-code-master-class = "claude_code_master_class:main"`. New CLI entry points belong there,
  and the target must be importable from the package root (re-export from `__init__.py` if the
  implementation lives in a submodule).
