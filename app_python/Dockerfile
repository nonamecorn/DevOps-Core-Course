FROM python:3.14-slim

# Prevents Python from writing pyc files & buffers stdout/stderr
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    POETRY_VERSION=2.3.2 \
    POETRY_VIRTUALENVS_CREATE=false

# Install Poetry
RUN pip install --no-cache-dir "poetry==$POETRY_VERSION"

# Create non-root user
RUN useradd --shell /bin/sh appuser

WORKDIR /app

# Copy only dependency files first (better layer caching)
COPY pyproject.toml poetry.lock* ./

# Install dependencies
RUN poetry install --no-interaction --no-ansi --no-root

# Copy project files
COPY ./src/ .

# Switch to non-root user
USER appuser

CMD ["python", "app.py"]
