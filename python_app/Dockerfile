# Multi‑arch friendly slim image
FROM python:3.10-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    FLASK_ENV=production \
    REDIS_HOST=redis \
    REDIS_PORT=6379 \
    REDIS_DB=0

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends curl \
    && rm -rf /var/lib/apt/lists/*

# Copy and install Python dependencies
COPY python_app/requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY python_app/ /app/

# Create non‑root user and assign ownership
RUN useradd -r -u 10001 appuser && chown -R appuser:appuser /app
USER appuser

EXPOSE 5000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD curl -fsS http://127.0.0.1:5000/ || exit 1

# Run the Flask app through Gunicorn
CMD ["gunicorn", "-w", "2", "-k", "gthread", "--threads", "4", "--timeout", "60", "-b", "0.0.0.0:5000", "app:app"]