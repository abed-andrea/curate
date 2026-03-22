FROM python:3.11-slim

WORKDIR /src

# copy and install dependencies first (layer caching - rebuilds skip this step if dependencies haven't changed)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# copy my /app into container (/src/app)
COPY app ./app

# start the server
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
