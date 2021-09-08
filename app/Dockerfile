FROM tiangolo/uvicorn-gunicorn-fastapi:python3.8-alpine3.10 as uvicorn

COPY . /app



FROM python:3.8-alpine3.10 as alpine

WORKDIR /app

RUN apk --update add --no-cache --virtual .build-deps \
    gcc \
    musl-dev \
    linux-headers \
    build-base \
    libffi-dev

COPY . .

# ENV PIPENV_VENV_IN_PROJECT=1

RUN pip install --upgrade pip && \
    pip install pipenv && \
    pipenv install --deploy --system

RUN apk del --purge .build-deps

CMD ["uvicorn", "main:app", "--reload", "--host", "127.0.0.1", "--port", "8000"]


FROM python:3.8.12-slim as slim

WORKDIR /app

COPY . .

RUN pip install --upgrade pip && \
    pip install pipenv && \
    pipenv install --deploy --system

CMD ["uvicorn", "main:app", "--reload", "--host", "127.0.0.1", "--port", "8000"]
