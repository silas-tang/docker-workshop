# Start with slim Python 3.13 image
FROM python:3.13.10-slim

# 配置代理
ENV HTTP_PROXY=http://172.19.160.1:7897
ENV HTTPS_PROXY=http://172.19.160.1:7897
ENV ALL_PROXY=http://172.19.160.1:7897

# Copy uv binary from official uv image (multi-stage build pattern)
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/

# Set working directory
WORKDIR /app

# Add virtual environment to PATH so we can use installed packages
ENV PATH="/app/.venv/bin:$PATH"

# Copy dependency files first (better layer caching)
COPY "pyproject.toml" "uv.lock" ".python-version" ./
COPY "src" "./src"
# Install dependencies from lock file (ensures reproducible builds)
RUN uv sync --locked
# Copy application code
COPY ingest_data.py .

# Set entry point
ENTRYPOINT ["python", "ingest_data.py"]