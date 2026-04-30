FROM mcr.microsoft.com/playwright/python:v1.54.0-jammy

WORKDIR /app

# 构建上下文 = 本仓库（ascend-skills-eval）根目录。
# Zeabur「Root Directory」留空；若误设为 web-service，需把下方 COPY 改为相对 web-service 的路径。
COPY web-service/requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir -r /app/requirements.txt

COPY web-service/app /app/app
COPY skills/skills-eval /app/skills/skills-eval

ENV PYTHONUNBUFFERED=1
ENV PORT=8000

EXPOSE 8000

CMD ["sh", "-c", "uvicorn app.main:app --host 0.0.0.0 --port ${PORT}"]
