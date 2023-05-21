
FROM python:3.11.3-slim as base

ENV PYTHONUNBUFFERED=true \
	PYTHONDONTWRITEBYTECODE=1 \
	POETRY_VIRTUALENVS_IN_PROJECT=true \
	POETRY_NO_INTERACTION=1 \
	PYSETUP_PATH="/opt/pysetup" \
	VENV_PATH="/opt/pysetup/.venv" \
	APPLICATION_DIR="/home/code/"

ENV PATH="$VENV_PATH/bin:$PATH"


FROM base as poetry-build

WORKDIR $PYSETUP_PATH

COPY poetry.lock pyproject.toml ./

RUN python -m pip install --upgrade pip setuptools wheel \
    && python -m pip install poetry \
    && poetry install --no-ansi --no-root -vvv


FROM base as app

WORKDIR $APPLICATION_DIR

COPY --from=poetry-build $VENV_PATH $VENV_PATH

COPY app/ ./app/

CMD ["uvicorn", "app.main:app", "--reload", "--host", "0.0.0.0", "--port", "80"]
