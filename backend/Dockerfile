FROM python:3.9.7-slim-buster

ENV APP_HOME=/app/backend
# Select the new directory as working directory
WORKDIR ${APP_HOME}

RUN groupadd --gid 1000 app && useradd --uid 1000 --gid app --shell /bin/bash --create-home app

ENV PYTHONFAULTHANDLER=1 \
    PYTHONUNBUFFERED=1 \
    PYTHONHASHSEED=random \
    PIP_NO_CACHE_DIR=off \
    PIP_DISABLE_PIP_VERSION_CHECK=on \
    PIP_DEFAULT_TIMEOUT=100 \
    POETRY_NO_INTERACTION=1 \
    POETRY_VIRTUALENVS_CREATE=false \
    POETRY_HOME=/usr/local \
    # Versions:
    POETRY_VERSION=1.1.11

## Install Poetry
RUN \
    pip install -U pip && \
    pip install "poetry==$POETRY_VERSION"

COPY poetry.lock ${APP_HOME}/poetry.lock
COPY pyproject.toml ${APP_HOME}/pyproject.toml
# Install dependencied
RUN poetry install
# Loads virtual env
RUN poetry shell

COPY ./backend ${APP_HOME}
# chown all the files to the app user
RUN chown -R app:app ${APP_HOME}
USER app

# Start Django Project
CMD ["bash", "./deploy.sh"]
# Expose server port
EXPOSE 8000
