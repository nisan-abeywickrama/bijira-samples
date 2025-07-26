# Use Python 3.11 as base image
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the entire project
COPY . .

# Install mcp-use package from GitHub
RUN pip install git+https://github.com/nisan-abeywickrama/bijira-samples.git#subdirectory=external-lib/mcp-use


# Create a new user with UID 10014
RUN addgroup --gid 10014 choreo && \
    adduser --disabled-password --no-create-home --uid 10014 --ingroup choreo choreouser

RUN chown -R choreouser:choreo /app

# Set a non-root user
USER 10014

# Expose port 5050
EXPOSE 5050

CMD ["gunicorn", "--bind", "0.0.0.0:5050", "app:app"]
