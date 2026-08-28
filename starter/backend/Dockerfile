FROM public.ecr.aws/docker/library/python:3.10-slim

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /app

RUN useradd -u 1000 -m app

RUN apt-get update && \
    apt-get install -y \
    build-essential \
    gcc \
    pkg-config \
    libpcre2-dev \
    libssl-dev \
    libffi-dev \
    && pip install --no-cache-dir pipenv \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /var/www/app

COPY Pipfile* /app/

RUN pipenv install --system --deploy

COPY . /app

USER app

CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/=app:app"]