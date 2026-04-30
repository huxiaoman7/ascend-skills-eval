FROM mcr.microsoft.com/playwright/python:v1.54.0-jammy

WORKDIR /app

# Install Python dependencies
COPY web-service/requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir -r /app/requirements.txt

# Copy only required files
COPY web-service/app /app/app
COPY skills/skills-eval /app/skills/skills-eval

ENV PYTHONUNBUFFERED=1
ENV PORT=8000

EXPOSE 8000

CMD ["sh", "-c", "uvicorn app.main:app --host 0.0.0.0 --port ${PORT}"]
