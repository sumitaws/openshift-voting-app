# Using official python runtime base image
FROM python:3.9-slim

# Add curl for healthcheck
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Set the application directory
WORKDIR /app

# Add PATH for local installs
ENV PATH="/app/.local/bin/:${PATH}"

# Create a user and set permissions
RUN useradd -u 1001 -r -g 0 -d /app -s /sbin/nologin \
    -c "Default Application User" default && \
    chown -R 1001:0 /app && \
    chmod -R g+rw /app

USER 1001

# Install requirements
COPY src/requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir -r /app/requirements.txt

# Copy application code
COPY src/. .

# Make port 8080 available
EXPOSE 8080

# Set the command to run when launching the container
CMD ["gunicorn", "app:app", "-b", "0.0.0.0:8080", "--log-file", "-", "--access-logfile", "-", "--workers", "4", "--keep-alive", "0"]
