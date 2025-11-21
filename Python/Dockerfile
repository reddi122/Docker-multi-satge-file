# ====== Stage 1: Build dependencies ======
FROM python:3.12-slim AS build
WORKDIR /app

# Install build tools
RUN apt-get update && apt-get install -y build-essential git && rm -rf /var/lib/apt/lists/*

# Clone repo (optional)
RUN git clone https://github.com/<username>/<repository>.git .

# Copy requirements.txt first for caching
COPY requirements.txt .
RUN pip install --prefix=/install -r requirements.txt

# Copy app code
COPY . .

# ====== Stage 2: Production ======
FROM python:3.12-slim
WORKDIR /app

# Copy installed packages from build stage
COPY --from=build /install /usr/local

# Copy app code
COPY --from=build /app .

# Expose port
EXPOSE 8000

# Run app (Flask example)
# CMD ["python", "app.py"]

# Or FastAPI + Uvicorn
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
