# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

[Brief description of what the application does, its primary calculation/visualization modes, and core technologies used]

## Quick Start

```bash
# Install dependencies
poetry install

# Run the application
streamlit run src/main.py

# Run tests
pytest tests/
```

## Code Architecture

**Functional Programming Principles:**
- Pure functions by default: no side effects, deterministic outputs
- Clear separation: UI setup → calculation logic → rendering
- Low coupling, high cohesion between modules
- Module names should describe their purpose (e.g., `vector_calculations.py`, `animation_renderer.py`)

**Code Quality Standards:**
- Type annotations for all function signatures including return types
- Use `enum.StrEnum` for string constants (e.g., calculation modes, shape types)
- Store numeric constants in `config.yaml` (no magic numbers)
- Function-level validation: check arguments early, fail fast with clear error messages
- Docstrings for public functions: brief description, parameter types/meanings, return value

**Project Structure:**
```
src/          # Application code
tests/        # Unit tests
logs/         # Log files
config.yaml   # Constants and configuration
```

## Logging

**Setup:**
- Module-level loggers: `logger = logging.getLogger(__name__)`
- Log to files in `logs/` directory with rotation
- Add custom filter to exclude dependency logs:
  ```python
  class SrcOnlyFilter(logging.Filter):
      def filter(self, record):
          return record.name.startswith('src.')
  ```

**Usage:**
- DEBUG: Internal state, calculation steps
- INFO: User actions, major state changes
- WARNING: Recoverable issues, fallback behavior
- ERROR: Failures that prevent specific operations

## Testing

**Coverage:**
- Unit tests for all calculation functions (especially edge cases)
- Property-based tests for vector math (use `hypothesis` if complex)
- UI integration tests using Playwright MCP server
- Test file naming: `test_<module_name>.py`

**Test Organization:**
- Group related tests in classes
- Use parametrize for multiple input scenarios
- Mock external dependencies and file I/O

## Visualization & Animation

**Matplotlib Animations:**
- Use `matplotlib.animation.FuncAnimation` for efficiency
- Bind callback arguments with `functools.partial` (avoid closure scope issues)
- Initialize figure/axes once, update data in animation function
- Example pattern:
  ```python
  fig, ax = plt.subplots()
  line, = ax.plot([], [])
  
  def update(frame, line, data):
      line.set_data(data[:frame])
      return line,
  
  anim = FuncAnimation(fig, partial(update, line=line, data=data), ...)
  ```

**Rendering Guidelines:**
- Fit visualizations within viewport (no scrolling/panning required)
- Set explicit axis limits based on data bounds
- Use `ax.set_aspect('equal')` for geometric accuracy when needed
- Clear axis labels and legends for educational clarity

## Domain-Specific Notes

**Coordinate Systems:**
[Describe any domain-specific coordinate transformations, e.g., nautical bearings → Cartesian]

**Calculation Modes:**
[List distinct calculation modes with brief input/output descriptions]

**Key Algorithms:**
[Note any non-obvious algorithms or solution selection criteria]

## Dependencies

- Python [State Python version. E.g. ≥3.13]
- Managed with [State which dependency manager you are using. E.g. Poetry (v2+)]
- Virtual environment: in-project (`.venv/`)
- Add new dependencies: `poetry add <package>`
