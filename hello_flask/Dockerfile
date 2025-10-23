# -------- Stage 1: Build Stage --------
FROM python:3.8-slim AS build

WORKDIR /app

# Install system packages needed to build mysqlclient
RUN apt-get update && apt-get install -y \
    gcc \
    python3-dev \
    libmariadb-dev \
    pkg-config \
 && rm -rf /var/lib/apt/lists/*

# Copy project files into the image
COPY . .

# Install Python dependencies (Flask + MySQL client)
RUN pip install --no-cache-dir flask mysqlclient


# -------- Stage 2: Production Stage --------
FROM python:3.8-slim

WORKDIR /app

# ✅ Copy app files from build stage
COPY --from=build /app /app

# ✅ Copy installed Python libraries from build stage
COPY --from=build /usr/local/lib/python3.8 /usr/local/lib/python3.8
COPY --from=build /usr/local/bin /usr/local/bin

# ✅ Install only the runtime MariaDB client (needed by mysqlclient)
RUN apt-get update && apt-get install -y --no-install-recommends \
    libmariadb3 \
 && rm -rf /var/lib/apt/lists/*

EXPOSE 5000

CMD ["python", "app.py"]
