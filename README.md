# llm-viz-freecodecamp
Small tutorial from freeCodeCamp

https://www.freecodecamp.org/news/how-llms-work-under-the-hood/

## Getting Started with `uv`

This project uses [uv](https://github.com/astral-sh/uv) for fast Python package management.

### Installation

If you don't have `uv` installed, you can install it via:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### Setup

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd llm-viz-freecodecamp
   ```

2. **Sync dependencies**:
   This will create a virtual environment and install all required packages.
   ```bash
   uv sync
   ```

### Running the Project

To run the Jupyter notebook:
```bash
uv run jupyter notebook llm-viz.ipynb
```

To run the main script:
```bash
uv run python main.py
```

