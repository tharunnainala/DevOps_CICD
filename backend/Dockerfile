FROM public.ecr.aws/docker/library/python:3.10-alpine

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /app

RUN apk add --no-cache \
    pcre \
    pcre-dev \
    build-base \
    gcc \
    linux-headers \
    python3-dev \
    musl-dev \
    openssl-dev \
    libffi-dev

RUN pip install --upgrade pip setuptools wheel

RUN pip install --no-cache-dir pipenv

COPY Pipfile* /app/

RUN pipenv install --system

RUN pip install --no-cache-dir uwsgi

COPY . /app

RUN adduser -u 1000 -D app && \
    chown -R app:app /app

USER app

CMD ["uwsgi", "--chdir", "/", "--http", ":5000", "--master", "--enable-threads", "--module", "app:app"]