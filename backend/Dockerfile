FROM python:3.10-slim-bullseye

WORKDIR /app

# Create a non-root user
RUN useradd -u 1000 -m app

# Install required build dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    build-essential \
    gcc \
    libpcre3-dev \
    libssl-dev \
    libffi-dev && \
    rm -rf /var/lib/apt/lists/*

# Install Pipenv
RUN python -m pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir pipenv

# Copy dependency files
COPY Pipfile Pipfile.lock ./

# Install Python dependencies
RUN pipenv install --system --deploy --ignore-pipfile

# Copy application source code
COPY . .

# Set ownership
RUN chown -R app:app /app

# Run as non-root user
USER app

CMD ["uwsgi", "--ini", "uwsgi.ini"]