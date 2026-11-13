# Socle Alpine (musl) plutôt que Debian slim : slim porte 209 vulnérabilités système (2
# critiques, 74 hautes) sans correctif publié (osv-scanner, 2026-11-13). Toutes les
# dépendances Python ont des roues musllinux.
FROM python:3.12-alpine
# Alpine publie ses correctifs plus vite que l'image python n'est reconstruite : on les
# applique (krb5 et zlib, 16 vulnérabilités dont 1 critique, le 2026-11-13).
RUN apk upgrade --no-cache

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app ./app

# pip ne sert qu'à la construction : on le retire de l'image finale (12 vulnérabilités dans
# celui du socle, dont sa copie de secours d'ensurepip ; un outil d'installation de moins).
RUN pip uninstall -y pip \
    && rm -f /usr/local/lib/python3.*/ensurepip/_bundled/*.whl

EXPOSE 8000

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
