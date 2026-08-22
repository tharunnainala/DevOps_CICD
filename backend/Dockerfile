FROM python:3.10-slim

WORKDIR /app

RUN useradd -u 1000 -m app

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    build-essential \
    libpcre3-dev \
    libssl-dev \
    libffi-dev && \
    rm -rf /var/lib/apt/lists/*

RUN python -m pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir pipenv

COPY Pipfile Pipfile.lock ./

RUN pipenv install --system --deploy --ignore-pipfile

COPY . .

RUN chown -R app:app /app

USER app

CMD ["uwsgi", "--ini", "uwsgi.ini"]