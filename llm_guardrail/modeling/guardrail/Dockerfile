FROM python:3.10-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_DISABLE_PIP_VERSION_CHECK=on \
    HF_HOME=/opt/hf_cache \
    TRANSFORMERS_CACHE=/opt/hf_cache

RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        git \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY requirements.txt .

RUN python -m pip install --upgrade pip \
    && pip install --no-cache-dir -r requirements.txt \
    && pip install --no-cache-dir "torch==2.3.1" --index-url https://download.pytorch.org/whl/cpu

COPY src ./src

RUN useradd -ms /bin/bash appuser \
    && mkdir -p ${HF_HOME} \
    && chown -R appuser:appuser /app ${HF_HOME}
USER appuser

EXPOSE 8001

CMD ["uvicorn", "src.app:app", "--host", "0.0.0.0", "--port", "8001"]