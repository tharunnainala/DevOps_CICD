FROM python:3.10-slim

# Set working directory
WORKDIR /app

# Create non-root user
RUN useradd -u 1000 -m app

# Install system dependencies
RUN apt-get update && \
    apt-get install -y \
    build-essential \
    gcc \
    libpcre3 \
    libpcre3-dev \
    libssl-dev \
    libffi-dev && \
    python -m pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir pipenv && \
    rm -rf /var/lib/apt/lists/*

# Copy dependency files first
COPY Pipfile Pipfile.lock ./

# Install Python dependencies
RUN pipenv install --system --deploy --ignore-pipfile

# Copy application files
COPY . .

# Give ownership to app user
RUN chown -R app:app /app

# Switch to non-root user
USER app

# Run the application
CMD ["uwsgi", "--ini", "uwsgi.ini"]