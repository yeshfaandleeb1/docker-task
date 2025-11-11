# Base image
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Install system dependencies (needed for psycopg2-binary)
RUN apt-get update && apt-get install -y \
    gcc \
    libpq-dev \
    python3-dev \
    && rm -rf /var/lib/apt/lists/*

# Copy dependency list
COPY requirements.txt .

# Install dependencies
RUN pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Copy all backend code
COPY . .

# Expose backend port
EXPOSE 8000

# Command to run your app (adjust as needed)
CMD ["python", "main.py"]
