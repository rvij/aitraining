# Repository Setup

The training exercises are built around a sample Python project. Clone it, install dependencies, and make sure the tests pass before you start.

## Checklist

- [ ] I have cloned the training repository
- [ ] Dependencies are installed
- [ ] The test suite passes locally
- [ ] I understand the project structure

## Clone the Repo

```bash
git clone https://github.com/your-org/altraining
cd altraining
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```

## Project Structure

```
altraining/
├── src/
│   ├── api/           # FastAPI application
│   ├── models/        # Data models
│   └── services/      # Business logic
├── tests/             # Pytest test suite
├── AGENTS.md          # AI assistant instructions
├── pyproject.toml
└── README.md
```

## Running Locally

```bash
# Run the dev server
uvicorn src.api.main:app --reload

# Run the test suite
pytest tests/ -v

# Run with coverage
pytest tests/ --cov=src --cov-report=term
```

!!! warning
    Make sure your virtual environment is activated before running any commands — otherwise you may use system Python and hit version conflicts.
