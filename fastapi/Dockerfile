FROM python:3.10-slim-bullseye

# Install system dependencies and SQL Server drivers
RUN apt-get update -qqy && \
    apt-get -qqy install sudo build-essential curl unixodbc unixodbc-dev && \
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo tee /etc/apt/trusted.gpg.d/microsoft.asc && \
    curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list | sudo tee /etc/apt/sources.list.d/mssql-release.list && \
    apt-get update && \
    sudo ACCEPT_EULA=Y apt-get install -y msodbcsql17 mssql-tools && \
    rm -rf /var/lib/apt/lists/*

# Add a non-root user and switch to that user
RUN adduser appuser && \
    echo "appuser ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
USER appuser
WORKDIR /home/appuser

# Install dependencies
COPY requirements.txt .
RUN python -m venv venv && \
    venv/bin/pip install --upgrade pip wheel setuptools && \
    venv/bin/pip install -r requirements.txt

# Copy application code
COPY app app
COPY entrypoints/server-entrypoint.sh .

# Make entrypoint script executable
RUN chmod +x server-entrypoint.sh

# Expose the FastAPI port
EXPOSE 5000

# Use the entrypoint script
CMD ["./server-entrypoint.sh"]

